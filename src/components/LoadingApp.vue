<template>
    <pulse-loader :loading="loading" :color="color" :size="size"></pulse-loader>
</template>
<script>
import PulseLoader from 'vue-spinner/src/PulseLoader.vue'
export default {
    props: {
        loading: {
            type: Boolean,
            default: true
        },
        color: {
            type: String,
            default: '#5dc596'
        },
        size: {
            type: String,
            default: '15px'
        },
        margin: {
            type: String,
            default: '2px'
        },
        radius: {
            type: String,
            default: '100%'
        }
    },
    components: {
        PulseLoader
    }
}
</script>
<style></style>