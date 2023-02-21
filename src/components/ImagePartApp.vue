<template>
    <div class="image-part">
        <img @load="onImgLoad" loading="lazy" :src="require(`@/assets/${imageSrc}`)" :alt="imageAlt">
        <loading-app :loading="!isLoaded"> </loading-app>
    </div>
</template>

<script>
import LoadingApp from './LoadingApp.vue';

export default {
    data() {
        return {
            isLoaded: false
        }
    },
    props: {
        id: Number,
        imageSrc: String,
        imageAlt: String
    },
    methods: {
        onImgLoad() {
            this.isLoaded = true
        }
    },
    components: {
        LoadingApp
    }
}
</script>

<style lang="scss">
.image-part {
    text-align: center;

    // height: 300px;
    // display: flex;
    // align-items: center;
    img {
        width: 70%;
        transition: all 1s ease-in;
        cursor: zoom-in;
        transform: scale(0.9);
        opacity: 0.8;
        // filter: brightness(50%);
        box-shadow: rgba(199, 199, 211, 0.74) -5px 5px, rgba(199, 199, 211, 0.3) -10px 10px, rgba(199, 199, 211, 0.2) -15px 15px, rgba(199, 199, 211, 0.1) -20px 20px, rgba(199, 199, 211, 0.05) -25px 25px;

        &:hover {
            transform: scale(1);
            opacity: 1;
        }
    }
}
</style>