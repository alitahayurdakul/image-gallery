<template>
    <div class="image-part">
        <div class="image-frame">
            <img @load="onImgLoad" loading="lazy" :src="require(`@/assets/${imageSrc}`)" :alt="imageAlt" @error="setFallbackImageUrl">
            <loading-app :loading="!isLoaded"> </loading-app>
        </div>
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
        },
        setFallbackImageUrl(event){
            event.target.src = "@/assets/avatar.webp";
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

    
    .image-frame {
        width: 80%;
        margin: 0 auto;
        box-shadow: rgba(0, 0, 0, 0.2) 0px 60px 40px -7px;
        transform: scale(0.9);
        transition: all .4s ease-in;
        border-radius: 10px;

        &:hover {
            transform: scale(1);
        }

        img {
            display: block;
            width: 100%;
            cursor: zoom-in;
            border-radius: 10px;
            // filter: contrast(110%) brightness(110%);
            -webkit-mask-image: linear-gradient(45deg, #000 25%, rgba(0, 0, 0, .2) 50%, #000 75%);
            mask-image: linear-gradient(45deg, #000 25%, rgba(0, 0, 0, .2) 50%, #000 75%);
            -webkit-mask-size: 800%;
            mask-size: 800%;
            -webkit-mask-position: 0;
            mask-position: 0;

            &:hover {

                transition: mask-position 2s ease, -webkit-mask-position 2s ease;
                -webkit-mask-position: 120%;
                mask-position: 120%;
                opacity: 1;
            }
        }
    }
}
</style>