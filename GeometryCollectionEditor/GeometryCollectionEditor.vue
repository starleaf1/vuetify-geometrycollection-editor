<template>
    <v-sheet>
        <v-list>
            <v-list-item v-for="(geometry, i) in geoJson.geometries" :key="geometry">
                <ShapeListItem :i="i" @deleteorder="deleteShape($event)" v-model="geoJson.geometries[i]" />
            </v-list-item>
        </v-list>
        <v-snackbar v-model="snackbarDisplay">
            Lokasi dihapus dari daftar.
            <v-btn color="yellow" text @click="returnShape">Kembalikan</v-btn>
        </v-snackbar>

        <v-btn dark color="blue" block @click="addShape">Tambah Lokasi</v-btn>
    </v-sheet>
</template>

<script>
import ShapeListItem from './ShapeListItem';

export default {
    components: {
        ShapeListItem
    },
    props: {
        value: {
            type: Object,
            default: () => ({
                type: "GeometryCollection",
                geometries: [{
                    type: "Point",
                    coordinates: [0, 0]
                }]
            }),
            validator: (v) =>  (
                v.type === "GeometryCollection" && v.geometries.length >= 1 && (
                    v.geometries.every(e => e.type === "Point" && e.coordinates.length === 2) ||
                    v.geometries.every(e => e.type === "LineString" && e.coordinates.length >= 2) ||
                    v.geometries.every(e => e.type === "Polygon" && e.coordinates[0].length === 2)
                )
            )
        },
    },
    data() {
        return {
            geoJson: this.$props.value,
            deletedShape: null,
            snackbarDisplay: false
        }
    },
    methods: {
        addShape() {
            this.$data.geoJson.geometries.push({
                type: "Point",
                coordinates: [0, 0]
            })
            this.emitValue()
        },
        deleteShape(i) {
            console.log(i);
            this.$data.deletedShape = {
                position: i,
                shape: this.$data.geoJson.geometries.splice(i, 1)[0]
            }
            
            this.$data.snackbarDisplay = true;
            this.emitValue()
        },
        returnShape() {
            this.$data.geoJson.geometries.splice(this.$data.deletedShape.position, 0, this.$data.deletedShape.shape)
            this.$data.snackbarDisplay = false;
            this.emitValue()
        },
        emitValue() {
            this.$emit('change', this.$data.geoJson)
        }
    }
}
</script>