<template>
    <v-list-item-content>
        <v-list-item-title>Lokasi {{i + 1}}</v-list-item-title>
        <v-list-item-subtitle>
            <span>{{geometry.type}}</span>
            <span v-if="geometry.type == 'LineString'"> {{geometry.coordinates.length}} titik</span>
            <span v-if="geometry.type == 'Polygon'"> {{geometry.coordinates[0].length}} titik</span>
        </v-list-item-subtitle>
        <v-list-item-action-text>
            <v-btn @click="$emit('deleteorder', i)" v-on="on" text color="red"><v-icon>mdi-trash</v-icon>Hapus</v-btn>
            <v-dialog persistent v-model="dialogOpen">
                <template v-slot:activator="{on}">
                    <v-btn v-on="on" text color="blue"><v-icon>mdi-pencil</v-icon>Edit</v-btn>
                </template>
                <v-card>
                    <ShapeEditor v-model="geometry" />
                    <v-card-actions>
                        <v-btn @click="dialogOpen = false" color="red" dark>Batal</v-btn>
                        <v-btn @click="confirmValue" color="green" :disabled="!validShape()" :dark="validShape()">OK</v-btn>
                    </v-card-actions>
                </v-card>
            </v-dialog>
        </v-list-item-action-text>
    </v-list-item-content>
</template>

<script>

import ShapeEditor from './ShapeEditor.vue';

export default {
    components: {
        ShapeEditor
    },
    props: {
        value: {
            type: Object,
            default: () => ({
                type: "Point",
                coordinates: [0, 0]
            })
        },
        i: Number
    },
    data() {
        return {
            dialogOpen: false,
            geometry: this.$props.value
        }
    },
    methods: {
        confirmValue() {
            this.$data.dialogOpen = false;
            this.$emit('change', this.$data.geometry)
        },
        validShape() {
            
            return (
                (
                    this.$data.geometry.type === 'Polygon' && 
                    this.$data.geometry.coordinates[0].length >= 4 && 

                    // Array comparison is such a pain in the ass
                    (
                        this.$data.geometry.coordinates[0][0][0] === this.$data.geometry.coordinates[0][this.$data.geometry.coordinates[0].length - 1][0] &&
                        this.$data.geometry.coordinates[0][0][1] === this.$data.geometry.coordinates[0][this.$data.geometry.coordinates[0].length - 1][1]
                    )
                ) || (
                    this.$data.geometry.type === 'LineString' &&
                    this.$data.geometry.coordinates.length >= 2
                ) || (
                    this.$data.geometry.type === 'Point' 
                )
            )
        },
    }
}
</script>