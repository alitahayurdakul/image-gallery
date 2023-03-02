<template>
    <div class="container">
        <div>
            <transition-group name="image-list" tag="ul" appear duration="1000">
                <li v-for="image in getImages" @click="onClickImage(image.id)" :key="image.id">
                    <image-part-app :id='image.id' :imageSrc='image.src' :imageAlt='image.alt'></image-part-app>
                </li>
            </transition-group>

            <div class="loading">
                <loading-app :loading="isLoading"></loading-app>
            </div>
            <div v-if="isWorking">
                <observe-app :isWork='isWorking' @intersect="intersected"></observe-app>
            </div>
            <!-- <div v-html="a"></div> -->
        </div>

        <transition name="pop-up" mode="out-in" appear>
            <div class="pop-up" v-if="isModal">
                <div class="back" @click="isModal = false"></div>
                <span @click="onClickPrevious" class="fas fa-less-than"></span>
                <img :src="require(`@/assets/${getImageSrc}`)" alt="current Image" title="current Image" />
                <span @click="onClickNext" class="fas fa-greater-than"></span>
            </div>
        </transition>
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
            imgId: null,
            // a: null
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
            if (this.page === 1) {
                await axios.get('http://localhost:3000/imageList')
                    .then(response => {
                        this.imageList = response.data;
                        this.page++;
                        this.images = [...this.images, ...this.imageList.slice(0, 15)];
                        this.isLoading = false;
                    })
                    .catch(err => console.log(err));

            }
            else if (this.page === 2) {
                this.page++;
                setTimeout(() => {
                    this.isWorking = false;
                    this.images = [...this.images, ...this.imageList.slice(15, this.imageList.length)];
                    this.isLoading = false;
                }, 500)
            }
            else {
                return null
            }

        },
        onClickImage(imgId) {
            this.isModal = true;
            this.imgId = imgId
        },
        onClickNext() {
            this.imgId === this.images.length ? this.imgId = 1 : this.imgId++;
        },

        onClickPrevious() {
            // this.imgId--;
            this.imgId === 1 ? this.imgId = this.images.length : this.imgId--;
        }
    },
    computed: {
        getImageSrc() {
            return this.images[this.imgId - 1].src
        },
        getImages() {
            return this.images;
        }
    },
    // created() {
    //     this.a = `${<observe-app :isWork='isWorking' @intersect="intersected"></observe-app>}`
    // },
}
</script>

<style lang="scss">
.container {
    padding: 20px 5%;
    width: 70%;
    margin: 20px auto 0;
    height: 700px;
    overflow-y: auto;
    background-color: white;
    border-radius: 10px;
    box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
    transition: all .4s ease;
    color: white;

    &:hover {
        color: rgb(58, 136, 238);
    }

    &::-webkit-scrollbar {
        width: 18px;
        cursor: pointer;
    }

    &::-webkit-scrollbar-track {
        background: transparent;
        border-radius: 3px;
    }

    &::-webkit-scrollbar-thumb {
        border: 5px solid rgba(0, 0, 0, 0);
        background-clip: padding-box;
        border-radius: 9999px;
        box-shadow: inset 0 0 0 10px;
        cursor: pointer;
    }

    ul {
        display: grid;
        gap: 60px 30px;
        grid-template-columns: repeat(3, 1fr);
    }

    .image-list-enter-active,
    .image-list-leave-active {
        transition: all .4s ease-in-out;

        .image-part {
            img {
                transition: all .4s ease-in-out;
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
        background-color: rgb(246, 246, 246) !important;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        cursor: zoom-out;
        justify-content: center;
        align-items: center;


        .back {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            z-index: -9999;
        }

        span {
            z-index: 1;
            cursor: pointer;
            font-size: 60px;
            margin-inline: 30px;
            color: rgba(0, 0, 0, 0.484);
            transition: color .4s;
            
            &:hover{
                color: black;
            }
        }

        img {
            z-index: 1;
            height: 400px;
            margin-block: auto;
            border-radius: 10px;
            filter: contrast(110%) brightness(110%);
            box-shadow: rgba(0, 0, 0, 0.25) 0px 54px 55px, rgba(0, 0, 0, 0.12) 0px -12px 30px, rgba(0, 0, 0, 0.12) 0px 4px 6px, rgba(0, 0, 0, 0.17) 0px 12px 13px, rgba(0, 0, 0, 0.09) 0px -3px 5px;
            cursor:auto;
        }



    }

    .pop-up-enter-active,
    .pop-up-leave-active {
        transition: all .4s ease-in-out;

        img {
            transition: all .4s ease-in-out;
        }
    }

    .pop-up-enter-from,
    .pop-up-leave-to {
        opacity: 0;

        img {
            transform: scale(0.8);
        }

    }

    .pop-up-enter-to,
    .pop-up-leave-from {
        opacity: 1;

        img {
            transform: scale(1);
        }
    }

}
</style>