<template>
  <div>
    <Header />
        <div v-for="product in products" :key="product.id" class="container py-2">
          <div class="row">
              <div class="shadow p-3 mb-5 bg-white rounded card col-sm-4 col-lg-4 mb-0 mb-lg-0 ">
                <div class="card-body">
                    <img v-bind:src="product.image" class="w-50 img-fluid rounded-3 mx-auto d-block" alt="image"/>
                    <h5 class="card-text mb-0">{{ product.name }}</h5>
                    <p class="card-text mb-0">{{ product.description }}</p> <br>
                    <h5 class="card-text text-dark mb-2">{{ product.price }}€</h5>
                    <button v-on:click="addOrderItem(product.id)" class="btn btn-outline-primary btn-lg mb-0" type="submit">
                       Add to cart
                    </button>
                  </div>
              </div>
          </div>
        </div>
    <Footer />
  </div>
</template>

<script>
import Footer from '@/components/Footer.vue'
import Header from '@/components/Header.vue'

export default {
    showModal: true,
    components: {
		Footer,
        Header
	},
	data() {
      return {
        products: [],
    }
	},
	mounted() {
    this.getProducts()
	},
  methods: {
    async getProducts() {
      if (await this.$store.dispatch('products/getProductsFromDB')) {
        this.products = this.$store.getters[('products/getProducts')]
      }
    },
    async addOrderItem(productId) {
      await this.$store.commit('basket/incrementProduct', productId)
    },
  },
  computed: {

  },
  created() {
  }
}

</script>

<style scoped>
</style>