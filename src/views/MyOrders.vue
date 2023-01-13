<template>
<div>
  
<Header />
  <div v-if="userLoggedIn" class="container-fluid">
    <div v-for="order in orders" :key="order.id">
      <div class="shadow p-3 mb-5 bg-white rounded card card m-2 p-1" style="width: 30rem;">
        <h3 class="card.title" > Order id {{ order.id}}</h3>
        <div class="card-body">
          <p class="card-text"> Data Created: {{ order.created_at }} </p>
          <p class="card-text"> Items: {{ order.items.length }} </p>
          <p class="card-text"> Total: {{ order.total }}€ </p>
        </div>
      </div>
    </div>
  </div><div v-else> <router-link type="btn" class="btn btn-dark btn-lg" to="/login">Login First
</router-link></div>

<Footer />

</div>
</template>

<script>
import Footer from '@/components/Footer.vue'
import Header from '@/components/Header.vue'


export default {
    components: {
		Footer,
        Header
	},
	data() {
		return {
            isHidden: false,
            id: 0,
            orders: 
            [
            //{
            //"id":"11",
            //"customer_id":"20",
            //"created_at":"2021-12-03 18:20:31",
            //"status_id":"1",
            //"total":"190",
            //"order_items":[{"product_id":"2","name":"Salmon Roll","price":"18","quantity":"4"},{"product_id":"3","quantity":"3"},{"product_id":"4","quantity":"2"}]
            //}
            ]
		}
	},
  mounted() {
    this.getUser()
    this.getOrders()
  },
  methods: {
    getUser() {
      this.user = this.$store.getters['user/getUser']
    },
    async getOrders() {
      if (await this.$store.dispatch('orders/getMyOrdersFromDB'))
        this.orders = this.$store.getters[('orders/getOrders')]
    },
  },
  computed: {
    userLoggedIn: function () {
      let user = this.$store.getters['user/getUser']
      for (var i in user) return true
      return false
    }
  }
}
</script>

<style scoped>

</style>