<template>
    <div class="md:col-span-9 col-span-12 p-4">
        <div class="item-post" v-for="post in listaPosts">
            <div class="my-4">
                <h1 class="text-4xl">{{ post.title }}</h1>
                <p>{{ post.body.substr(0, 100) + '...' }}</p>
                <a :href="`post/${post.id}`" class="text-red-500">Ler Mais</a>
            </div>
        </div>
        <div id="pagination" class="flex justify-center">
            <ul id="pagination-pessoas" class="pagination-sm pagination"></ul>
        </div>
    </div>
</template>
  
<script>
export default {
    head() {
        return {
            link: [
                { rel: 'stylesheet', href: '/css/bootstrap.min.css' }
            ],
            script: [
                {
                    src: "https://cdnjs.cloudflare.com/ajax/libs/jquery/3.1.1/jquery.min.js",
                    type: "text/javascript",
                    body: true,
                },
                {
                    src: "/js/jquery.twbsPagination.min.js",
                    type: "text/javascript",
                    body: true,
                }
            ]
        }
    },

    data() {
        return {
            listaPosts: []
        }
    },

    mounted() {
        this.pegarPosts();
    },

    methods: {
        pegarPosts() {
            this.$axios.$get('https://dummyjson.com/posts')
                .then((response) => {
                    this.listaPosts = response.posts.reverse();
                })
                .catch((error) => {
                    console.error(error);
                })
                .then(() => {
                    let totalItems = $('.item-post').length;
                    let itemPorPage = 10;

                    $('.item-post:gt(' + (itemPorPage - 1) + ')').attr('style', 'display: none !important');

                    $('#pagination-pessoas').twbsPagination({
                        totalPages: Math.ceil(totalItems / itemPorPage),
                        visiblePages: 3,
                        next: '›',
                        prev: '‹',
                        first: false,
                        last: false,
                        onPageClick: function (event, page) {
                            $('.item-post').attr('style', 'display: none !important');

                            let fim = itemPorPage * page;
                            let inicio = fim - itemPorPage;

                            for (let i = inicio; i < fim; i++) {
                                $('.item-post:eq(' + i + ')').attr('style', '');
                            }
                        }
                    });
                })
        }
    }
}
</script>