<template>
<div>
  <Header />
    <div class="container py-5 "  >
      <div class="row d-flex justify-content-center align-items-center h-100">
        <div class="col-10">

          <!-- Shopping Cart -->
          <div class="d-flex justify-content-between align-items-center mb-4">
            <h3 class="fw-normal mb-0 text-black">Shopping Cart</h3>
          </div>
          <div v-if="basket.length !== 0">
          <div v-for="product in products" :key="product.id" class="card rounded-3 mb-4">

            <div class="card-body p-4">
              <div class="row d-flex justify-content-between align-items-center">
                <div class="col-2 col-lg-2 col-xl-2">
                  <img v-bind:src="product.image" class="img-fluid rounded-3 mx-auto d-block" alt="image"/>

                </div>
                <div class="col-md-3 col-lg-3 col-xl-3">
                  <p class="lead fw-normal mb-2">{{ product.name }}</p>
                  <p><span class="text-muted">{{ product.description}}</span></p>
                </div>
                <div class="col-md-3 col-lg-3 col-xl-2 d-flex">
                  <button @click="subItem(product.id)" type="submit" class="btn btn-link px-2" >
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-dash-lg" viewBox="0 0 16 16">
                      <path fill-rule="evenodd" d="M2 8a.5.5 0 0 1 .5-.5h11a.5.5 0 0 1 0 1h-11A.5.5 0 0 1 2 8Z"/>
                    </svg>
                  </button>
                  {{ getQuantity(product.id) }}
                  <button @click="addItem(product.id)" type="submit" class="btn btn-link px-2" >
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-plus-lg" viewBox="0 0 16 16">
                      <path fill-rule="evenodd" d="M8 2a.5.5 0 0 1 .5.5v5h5a.5.5 0 0 1 0 1h-5v5a.5.5 0 0 1-1 0v-5h-5a.5.5 0 0 1 0-1h5v-5A.5.5 0 0 1 8 2Z"/>
                    </svg>
                  </button>
                </div>
                <div class="col-md-3 col-lg-2 col-xl-2 offset-lg-1">
                  <h5 class="mb-0">{{ product.price }}€</h5>
                </div>
              </div>
            </div>
          </div>
          <h5 class="mb-3">Total: {{ nCart }}€</h5>
          </div>
          <div v-if="userLoggedIn">
            <button v-on:click="createOrder()" type="submit" class="btn btn-primary btn-block btn-lg mx-2">Proceed Order</button>
            <button @click="clearBasket()" type="submit" class="btn btn-outline-danger btn-block btn-lg"> Clear Basket
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash-fill" viewBox="0 0 16 16">
                <path d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0z"/>
              </svg>
            </button>
          </div>
          <div v-else><button  @click="createOrder()" type="submit" class="btn btn-primary btn-block btn-lg mx-2 disabled">Proceed Order</button>
            <button @click="clearBasket()" type="submit" class="btn btn-outline-danger btn-block btn-lg"> Clear Basket
              <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash-fill" viewBox="0 0 16 16">
                <path d="M2.5 1a1 1 0 0 0-1 1v1a1 1 0 0 0 1 1H3v9a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2V4h.5a1 1 0 0 0 1-1V2a1 1 0 0 0-1-1H10a1 1 0 0 0-1-1H7a1 1 0 0 0-1 1H2.5zm3 4a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 .5-.5zM8 5a.5.5 0 0 1 .5.5v7a.5.5 0 0 1-1 0v-7A.5.5 0 0 1 8 5zm3 .5v7a.5.5 0 0 1-1 0v-7a.5.5 0 0 1 1 0z"/>
              </svg>
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
  name: 'products',
  components: {
    Footer,
    Header
  },
  data() {
    return {
      basket: [],
      products: [],
    }
  },
  mounted() {
    this.getBasket()
    this.getProduct()
  },
  methods: {
    async getProduct() {
      if (await this.$store.dispatch('products/getProductsFromDB')) {
        this.products = this.$store.getters['products/getProducts']
      }
      let array = []
      this.basket.forEach(item => {
        array.push(item.id)
      });
      this.products = this.products.filter(d => array.includes(d.id))
    },
    async getBasket() {
      this.basket = this.$store.getters['basket/getProducts']
      for (const value of this.basket) {
        this.products.push(await this.$store.getters['products/getProduct'](value.id))
      }
    },
    async addItem (productId) {
      await this.$store.commit('basket/incrementProduct', productId)
      this.basket.items = await this.$store.getters['basket/getProducts']
    },
    async subItem (productId) {
      await this.$store.commit('basket/decrementProduct', productId)
      this.basket.items = await this.$store.getters['basket/getProducts']
      this.getProduct()
    },
    async clearBasket() {
      await this.$store.commit('basket/clearBasket')
      this.basket = await this.$store.getters['basket/getProducts']
    },
    getQuantity(productId) {
      let result = this.basket.find(b => b.id == productId)
      if (typeof result === 'undefined') {
        return 0
      }
      return result.quantity
    },
    getTotalPrice(id) {
      let quantity = this.getQuantity(id)
      let result = this.products.find(p => p.id == id)
      return quantity*result.price
    },
    async createOrder() {
      var order = {status_id: 3, totalAmount: this.nCart, items: this.basket}
      if (await this.$store.dispatch('orders/addOrder', order)) {
        await this.$store.commit('basket/clearBasket')
        this.basket = await this.$store.getters['basket/getProducts']
        await this.$router.push('/message/1')
      }
    }

  },
  computed: {
    nCart: function(){
      let sum = 0
      for (let i = 0; i < this.products.length; i++) {
        sum += this.getTotalPrice(this.products[i].id)
      }
      return sum
    },
    userLoggedIn: function () {
      let user =  this.$store.getters['user/getUser']
      for (var i in user) return true
      return false
    },
  },
}
</script>

<style scoped>

</style>