# vuetify-geometrycollection-editor
A custom component for editing GeometryCollection in a GeoJSON

## Requirements
- Vuex
- Vuetify 2

## Installation
1. Download and extract to `components` directory.
2. Import `@/components/GeometryCollectionEditor/GeometryCollectionEditor.vue` into your component or view.
3. Attach a `v-model` to it, or listen for a `change` event.

## Needs fixing
- Internationalization.
- Value validation (it's currently possible to inject an invalid value via the `value` prop.
- Overall UX.
- Better testing methodologies.
