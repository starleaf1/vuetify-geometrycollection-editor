<template>
    <v-container fluid>
        <v-row>
            <v-col cols="12" align="center" justify="center">
                <GeoTypePicker @input="changeGeoType($event)" v-model="geometry.type" />
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12" align="center" justify="center">
                <v-list dense v-if="geometry.type === 'Point'">
                    <v-list-item>
                        <v-list-item-content><Coordinate @input="emitValue" v-model="geometry.coordinates" /></v-list-item-content>
                    </v-list-item>
                </v-list>
                <v-list dense v-else-if="geometry.type === 'Polygon'">
                    <v-list-item v-for="(coordinates, i) in geometry.coordinates[0]" :key="i">
                        <v-list-item-content><Coordinate @input="emitValue" v-model="geometry.coordinates[0][i]" /></v-list-item-content>
                        <v-list-item-action>
                            <v-tooltip top>
                                <template v-slot:activator="{ on }">
                                    <v-btn v-on="on" rounded @click="removePoint(i)"><v-icon>mdi-map-marker-off</v-icon></v-btn>
                                </template>
                                <span>Hapus titik ini</span>
                            </v-tooltip>
                        </v-list-item-action>
                    </v-list-item>
                </v-list>
                <v-list dense v-else>
                    <v-list-item v-for="(coordinates, i) in geometry.coordinates" :key="i">
                        <v-list-item-content>
                            <Coordinate @input="emitValue" v-model="geometry.coordinates[i]" />
                        </v-list-item-content>
                        <v-list-item-action>
                            <v-tooltip top>
                                <template v-slot:activator="{ on }">
                                    <v-btn v-on="on" rounded @click="removePoint(i)"><v-icon>mdi-map-marker-off</v-icon></v-btn>
                                </template>
                                <span>Hapus titik ini</span>
                            </v-tooltip>
                        </v-list-item-action>
                    </v-list-item>
                </v-list>
            </v-col>
        </v-row>
        <v-row v-if="geometry.type !== 'Point'">
            <v-col cols="12" align="center" justify="center"><v-btn @click="addPoint"><v-icon>mdi-map-marker-plus</v-icon> Tambahkan titik</v-btn></v-col>
        </v-row>
        <!-- <v-row class="mt-6">
            <v-spacer/>
            <v-col cols="4" align="center" justify="center">
                <v-btn block color="red" dark>Batal</v-btn>
            </v-col>
            <v-spacer />
            <v-col cols="4" align="center" justify="center">
                <v-btn :disabled="!validShape()" :dark="validShape()" block color="green">OK</v-btn>
            </v-col>
            <v-spacer/>
        </v-row> -->
    </v-container>
</template>

<script>
import Coordinate from "./Coordinate.vue";
import GeoTypePicker from "./GeoTypePicker.vue";

export default {
    components: {
        Coordinate, GeoTypePicker
    },

    props: {
        value: {
            type: Object,
            default: () => ({ type: "Point", coordinates: [0, 0] })
        }
    },

    data() {
        return {
            geometry: this.$props.value,
            menu: false
        }
    },
    computed: {
        
    },
    methods: {
        emitValue() {
            this.$emit('change', this.$data.geometry)
        },
        changeGeoType(v) {
            this.$data.geometry.coordinates = v === "Point" ? [0, 0] : [[0, 0]];
            this.$data.geometry.coordinates = v === "Polygon" ? [[[0, 0], [0, 0]]] : [[0, 0]];
            this.emitValue()
        },
        addPoint() {
            if (this.$data.geometry.type !== 'Point') {
                if (this.$data.geometry.type === 'LineString') this.$data.geometry.coordinates.push([0, 0])
                if (this.$data.geometry.type === 'Polygon') this.$data.geometry.coordinates[0].push([0, 0])
                this.emitValue()
            }
        },
        removePoint(i) {
            if (this.$data.geometry.type === "Polygon") {
                this.$data.geometry.coordinates[0].splice(i, 1)
            } else {
                this.$data.geometry.coordinates.splice(i, 1)
            }
            this.emitValue()
        }
    }
}
</script>