<template>
    <v-menu>
        <template v-slot:activator="{on}">
            <v-btn color="blue" dark width="300" v-on="on">{{buttonValue}} <v-icon>mdi-arrow-drop-down</v-icon></v-btn>
        </template>
        <v-list>
            <v-list-item @click="changeValue(item.title)" two-line v-for="(item, i) in items" :key="i">
                <v-list-item-content>
                    <v-list-item-title>{{item.title}}</v-list-item-title>
                    <v-list-item-subtitle>{{item.content}}</v-list-item-subtitle>
                </v-list-item-content>
            </v-list-item>
        </v-list>
    </v-menu>
</template>

<script>
export default {
    props: {
        "value": String
    },
    data() {
        return {
            items: [
                {title: "Point", content: "Geometri titik biasa."},
                {title: "LineString", content: "Cocok untuk pipa, jalan, pagar, dan sejenisnya."},
                {title: "Polygon", content: "Geometri bidang. Cocok untuk tanah, wilayah, bangunan, dan sejenisnya."}
            ],
            buttonValue: !this.$props.value ? "Pilih tipe geometri" : this.$props.value,
        }
    },
    methods: {
        changeValue(v) {
            this.$data.buttonValue = v;
            this.$emit('input', v)
        }
    }
}
</script>