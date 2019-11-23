<template>
    <v-container>
        <v-row>
            <v-col cols="6"><v-text-field :readonly="readonly" validate-on-blur :value="value[1]" @input="changeValue(1, $event)" label="Garis Lintang" type="number" /></v-col>
            <v-col cols="6"><v-text-field :readonly="readonly" validate-on-blur :rules="[rules.longitudeRule]" :value="value[0]" @input="changeValue(0, $event)" label="Garis Bujur" type="number" /></v-col>
        </v-row>
    </v-container>
</template>

<script>
export default {
    props: {
        value: {
            type: Array,
            default: () => [0, 0]
        },
        readonly: {
            type: Boolean,
            default : () => false
        }
    },
    data() {
        return {
            arrayValues: this.$props.value,
            rules: {
                longitudeRule: (v) => (!!parseFloat(v) && parseFloat(v) >= -90 && parseFloat(v) <= 90),
                latitudeRule: (v) => (!!parseFloat(v) && parseFloat(v) >= -180 && parseFloat(v) <= 180)
            }
        }
    },
    methods: {
        changeValue(index, value) {
            this.$data.arrayValues[index] = parseFloat(value)
            this.$emit('input', this.$data.arrayValues)
        }
    }
}
</script>