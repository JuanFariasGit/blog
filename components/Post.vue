<template>
    <div class="col-span-9 p-4">
        <h1 class="text-4xl">{{ post.title }}</h1>
        <p>{{ post.body }}</p>
    </div>
</template>

<script>
export default {
    data() {
        return {
            post: {},
            id: this.$route.params.id
        }
    },

    mounted() {
        this.pegarPost();
    },

    methods: {
        pegarPost() {
            this.$axios.$get(`https://jsonplaceholder.typicode.com/posts/${this.id}`)
                .then((response) => {
                    this.post = response;
                })
                .catch((error) => {
                    console.log(error);
                })
        }
    }
}
</script>