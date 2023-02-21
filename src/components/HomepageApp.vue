<template>
    <div class="container">
        <transition-group name="image-list" tag="ul" appear duration="1000">
            <li v-for="image in images" @click="onClickImage(image.src)" :key="image.id">
                <image-part-app :id='image.id' :imageSrc='image.src' :imageAlt='image.alt'></image-part-app>
            </li>
        </transition-group>

        <div class="loading">
            <loading-app :loading="isLoading"></loading-app>
        </div>
        <!-- If we click out of image, popup will close. -->
        <!-- <div class="pop-up" v-if="isModal">
                <div class="back" @click="isModal = false"></div>
                <img :src="require(`@/assets/${this.imgSrc}`)" alt="current Image" title="current Image"
                    @click="isModal = true" />
            </div> -->
        <div class="pop-up" v-if="isModal" @click="isModal = false">
            <img :src="require(`@/assets/${this.imgSrc}`)" alt="current Image" title="current Image" />
        </div>

        <observe-app :isWork='isWorking' @intersect="intersected"></observe-app>
    </div>
</template>
<script>
import ImagePartApp from './ImagePartApp.vue';
import axios from 'axios';
import Observe from './ObserverApp.vue';
import Loading from './LoadingApp.vue';

export default {
    data() {
        return {
            images: [],
            imageList: [],
            isWorking: true,
            page: 1,
            isLoading: false,
            isModal: false,
            imgSrc: ''
        }
    },
    components: {
        ImagePartApp,
        ObserveApp: Observe,
        LoadingApp: Loading
    },
    methods: {
        async intersected() {
            this.isLoading = true;
            setTimeout(() => {
                this.page === 2 ? this.isWorking = false : null;
                this.images = [...this.images, ...this.imageList.slice(15, this.imageList.length)];
                this.page++;
                this.isLoading = false
            }, 1000)
        },
        onClickImage(currentImage) {
            this.isModal = true;
            this.imgSrc = currentImage;
        }
    },
    async created() {
        await axios.get('http://localhost:3000/imageList')
            .then(response => {
                this.imageList = response.data;
                this.images = response.data.slice(0, 15)
            })
            .catch(err => console.log(err));
    }
}
</script>

<style lang="scss">
.container {
    padding: 20px 5%;
    height: 700px;
    overflow-y: auto;

    ul {
        display: grid;
        gap: 60px 30px;
        grid-template-columns: repeat(3, 1fr);
    }

    .image-list-enter-active,
    .image-list-leave-active {
        transition: all 1s ease-in-out;

        .image-part {
            img {
                transition: all 1s ease-in-out;
            }
        }

    }

    .image-list-enter-from,
    .image-list-leave-to {
        .image-part {
            img {
                opacity: 0;
            }
        }

    }

    .image-list-enter-to,
    .image-list-leave-from {
        .image-part {
            img {
                opacity: 1;
            }
        }

    }

    ul {
        list-style: none;
    }

    .loading {
        text-align: center;
        width: 100%;
    }

    .pop-up {
        display: flex;
        position: fixed;
        background-color: rgb(106, 105, 105);
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        cursor: zoom-out;
        justify-content: center;
        align-items: center;

        // .back {
        //     position: absolute;
        //     top: 0;
        //     left: 0;
        //     right: 0;
        //     bottom: 0;
        //     z-index: -9999;
        // }

        img {
            z-index: 1;
            width: 50%;
            margin-block: auto;
        }
    }
}
</style>