<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
     <div class="hero-body has-text-centered">
         <p class="title mb-6">
             Welcome to Poesy
         </p>
         <p class="subtitle">
             Gift Them Your Heart
         </p>
     </div>
    </section>

    <div class="column is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">Latest Product</h2>
      </div>

      <productreuse
      v-for="product in latestProducts"
      v-bind:key="product.id"
      v-bind:product="product" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'

import productreuse from '@/components/productreuse'

export default {
  name: 'Home',
  data() {
    return {
      latestProducts: []
    }
  },
  components: {  
    productreuse
  },
  mounted() {
    this.getLatestProducts()

    document.title = 'Home | Poesy'
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading',true)

      await axios
        .get('/api/v1/latest-products')
        .then(response => (
          this.latestProducts = response.data
        ))
        .catch(error => {
          console.log(error)
        })
      this.$store.commit('setIsLoading',false)
    }
  }
}
</script>

