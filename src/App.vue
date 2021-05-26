<template>
  <div id="wrapper">
    <nav class="navbar is-dark">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item"
          ><strong>DjanGoRide</strong></router-link
        >

        <a
          class="navbar-burger"
          aria-label="menu"
          aria-expanded="false"
          data-target="navbar-menu"
          @click="showMobileMenu = !showMobileMenu"
        >
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>
      
<!-- vue directive v-bind:class="{'is-active': showMobileMenu}"-->
      <div class="navbar-menu" id="navbar-menu" v-bind:class="{'is-active': showMobileMenu}">
        <div class="navbar-end">
          <router-link to="/bicicletas" class="navbar-item"
            >Bicicletas</router-link
          >
          <router-link to="/accesorios" class="navbar-item"
            >Accesorios</router-link
          >

          <div class="navbar-item">
            <div class="buttons">
              <router-link to="/log-in" class="button is-light">
                Log in</router-link
              >

              <router-link to="/carrito" class="button is-success">
                <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                <span>Carrito ({{ cartTotalLength }})</span>
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <section class="section">
    <router-view />
    </section>

    <footer class="footer">
      <p class="has-text-centered"> Proyecto para RIA - Copyright (c) 2021</p>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showMobileMenu: false,
      cart: {
        items: []
      }
    }
  },
  beforeCreate() {
    this.$store.commit('initStore')
  },
  mounted(){
    this.cart = this.$store.state.cart
  },
  computed: {
    cartTotalLength() {
      let totalLength = 0

      for (let i = 0; i < this.cart.items.length; i++){
        totalLength += this.cart.items[i].quantity
      }

      return totalLength
    }
  }
}
</script>

<style lang="scss">
@import "../node_modules/bulma";
</style>
