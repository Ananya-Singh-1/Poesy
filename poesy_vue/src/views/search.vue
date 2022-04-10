<template>
    <div class="page-search">
        <div class="columns is-multiline">
            <div class="column is-20">
                <h1 class="title">Search</h1>

                <h2 class="is-size-4 has-text-grey">Search term: "{{ query }}"</h2>
            </div>

            <productreuse 
                v-for="product in products"
                v-bind:key="product.id"
                v-bind:product="product" />
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import productreuse from '@/components/productreuse.vue'
export default {
    name: 'Search',
    components: {
        productreuse
    },
    data() {
        return {
            products: [],
            query: ''
        }
    },
    mounted() {
        document.title = 'Search | Poesy'
        let uri = window.location.search.substring(1)
        let params = new URLSearchParams(uri)
        if (params.get('query')) {
            this.query = params.get('query')
            this.performSearch()
        }
    },
    methods: {
        async performSearch() {
            this.$store.commit('setIsLoading', true)
            await axios
                .post('/api/v1/products/search/', {'query': this.query})
                .then(response => {
                    this.products = response.data
                })
                .catch(error => {
                    console.log(error)
                })
            this.$store.commit('setIsLoading', false)
        }
    }
}
</script>