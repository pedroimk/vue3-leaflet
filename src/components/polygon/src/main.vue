<template>
    <div style="display: none">
        <slot v-if="ready" />
    </div>
</template>

<script>
    import Options from '../../../mixins/Options.js';
    // import { polygon, extend } from 'leaflet';
    // import * as L from 'leaflet';
	import L from 'leaflet';
    const polygon = L.polygon;
    const extend = L.extend;


    export default {
        name: 'LPolygon',
        inject: ['lMap'],
        mixins: [Options],
        inheritAttrs: false,
        props: {
            latlngs: {
                type: [Array, Object],
                default: null,
            },
            isFitBounds: {
                type: Boolean,
                default: false
            }
        },
        data() {
            return {
                originOptions: {}
            };
        },
        methods: {
            initLeafletObject() {
                this.selfOptions = extend(this.originOptions, this.options, this.$attrs);
                this.self = polygon(this.latlngs, this.selfOptions);

                this.initFunction();

                this.$nextTick(() => {
                    this.isFitBounds && this.lMap.self.fitBounds(this.self.getBounds());
                });
            },

        },
        mounted() {
            this.initLeafletObject();
        }
    }
</script>
