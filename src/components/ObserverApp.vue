<template>
    <div class="observer" v-if="isWork" />
</template>
  
<script>
export default {
    props: {
        isWork: {
            default: true
        },
    }
    ,
    data: () => ({
        observer: null,
    }),
    mounted() {
        //  const options = this.options || {};
        const options = {
            root: document.querySelector("#scrollArea"),
            rootMargin: "0px",
            threshold: 1.0
        };
        this.observer = new IntersectionObserver(([entry]) => {
            if (entry && entry.isIntersecting) {
                this.$emit("intersect");
            }
        }, options);

        this.observer.observe(this.$el);
    },
    unmounted() {
        this.observer.disconnect();
    },
};
</script>