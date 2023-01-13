<template>
  <!-- Navbar -->
  <div class="navbar navbar-expand-lg navbar-dark">
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/menu">iShowPay</router-link>

      <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
              data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false"
              aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse justify-content-end" id="navbarSupportedContent">
        <ul class="navbar-nav">
          <li class="nav-item"><router-link class="nav-link active rounded-3" aria-current="page" to="/menu">Home</router-link></li>
          <li v-if="userLoggedIn" class="nav-item"><router-link class="nav-link active rounded-3" aria-current="page" to="/myorders">My Orders</router-link></li>
          <li v-if="userLoggedIn" class="nav-item"><router-link class="nav-link active rounded-3" aria-current="page" to="/basket">Basket</router-link></li>
          <li v-if="!userLoggedIn" class="nav-item"> <router-link class="nav-link active rounded-3" aria-current="page" to="/login">Login</router-link></li>
          <li v-else class="nav-item"> <router-link class="nav-link active rounded-3" aria-current="page" to="/logout">Logout</router-link></li>
          <li v-if="!userLoggedIn" class="nav-item"> <router-link class="nav-link active rounded-3" aria-current="page" to="/register">Register</router-link></li>
          <a class="nav-link"  href="#"  aria-expanded="false">Welcome {{ name }} </a>
         <router-link to="/basket" ><button class="btn btn-primary"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-cart" viewBox="0 0 16 16">
            <path d="M0 1.5A.5.5 0 0 1 .5 1H2a.5.5 0 0 1 .485.379L2.89 3H14.5a.5.5 0 0 1 .491.592l-1.5 8A.5.5 0 0 1 13 12H4a.5.5 0 0 1-.491-.408L2.01 3.607 1.61 2H.5a.5.5 0 0 1-.5-.5zM3.102 4l1.313 7h8.17l1.313-7H3.102zM5 12a2 2 0 1 0 0 4 2 2 0 0 0 0-4zm7 0a2 2 0 1 0 0 4 2 2 0 0 0 0-4zm-7 1a1 1 0 1 1 0 2 1 1 0 0 1 0-2zm7 0a1 1 0 1 1 0 2 1 1 0 0 1 0-2z"></path>
          </svg> {{nCart()}} </button> </router-link>
        </ul>
      </div>
    </div>
  </div>
  <!-- End Navbar -->

</template>

<script>
export default {
  data() {
    return {
      user: {
        id: '',
        name: '',
        email: '',
        session_id: ''
      },
      name: ''
    }
  },
  mounted() {
    this.ifUserloggedIn();

  },
  methods: {
    async ifUserloggedIn() {
      if(this.userLoggedIn) this.name = this.user.name

    },
    getBasket() {
      this.items = this.$store.getters['basket/getProducts']
    },
    nCart(){
      this.getBasket()
      let number = 0;
      this.items.forEach(item => {
        number += Number(item.quantity)
      });
      return number
    },
    getUser() {
      this.user = this.$store.getters['user/getUser']
    }
  },
  computed: {
    userLoggedIn: function () {
      this.getUser()
      for (var i in this.user) return true
      return false
    },
  }
}
</script>

<style>
.navbar {
  background-color: #0D4C92;
  color: white;
}
.dropdown-menu{
  background-color: #0D4C92;
  color:  black;
}

.dropdown-menu:hover{
  background-color: #0D4C92;
  color:  black;
}

.nav-link:hover{
  color: black;
}
</style>