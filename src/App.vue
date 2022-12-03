<template>
  <div id="app">
    <div class="container mt-4">
      <div class="nav-bar d-flex justify-content-between px-5 align-items-center">
        <div class="container">
          <img src="../src/assets/logo.png" class="logo" alt="vue_logo" />
        </div>
        <div class="cart d-flex">
          <div class="total-cart d-flex justify-content-center align-items-center me-1">
            <span class="text-light totalCart" @click="toggleModal">{{
                "Total: " + totalCart
            }}</span>
          </div>
          <div class="d-flex ps-5 border-start">
            <div class="cartLogo" @click="toggleModal">
              <h2><i class="bi bi-cart3 text-dark"></i></h2>
            </div>
            <div @click="toggleModal">
              <span class="cartNumber">{{ cartItemsNumber }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <productdisplay @add-to-cart="updateCart" @remove-from-cart="removeCart" :cart="cart" :primium="primium"
    :totalCart="total" />
  <div>
    <modalitem title="Cart" v-if="showModal" @deleteProduct="removeCart" @closeModal="toggleModal" :cart="cart"
      :totalCart="totalCart" :tax="tax" />
  </div>
  <signUp />
</template>

<script>
import modalitem from "./components/modal-item.vue";
import productdisplay from "./components/productDisplay.vue";
import signUp from "./components/sign-up.vue";
export default {
  name: "App",
  components: {
    productdisplay,
    modalitem,
    signUp,
  },
  data() {
    return {
      cart: [],
      primium: true,
      total: 0,
      showModal: false,
    };
  },
  methods: {
    updateCart(currentVariant) {
      //this.cart++;
      this.cart.push(currentVariant);
      this.total += currentVariant.price;
    },
    removeCart(currentVariant) {
      //this.cart--;
      let index = this.cart.indexOf(currentVariant);
      if (index >= 0) {
        this.cart.splice(index, 1);
        this.total -= currentVariant.price;
        if (this.total < 0) {
          this.total = 0;
        }
      }
    },
    toggleModal() {
      this.showModal = !this.showModal;
    },
  },
  computed: {
    cartItemsNumber() {
      return this.cart.length;
    },
    totalCart() {
      return this.total + " DT";
    },
    tax() {
      return this.total * 0.19 + " DT";
    },
  },
};
</script>

<style>
body {
  background-color: #eeeeee;
  margin: 0px;
  color: #282828;
}

.nav-bar {
  background: linear-gradient(-90deg, #84cf6a, #16c0b0);
  box-shadow: 5px 5px 19px #8f8f8f, -5px -5px 19px #ffffff;
  height: 60px;
  border-radius: 10px;
}

.logo {
  max-width: 40px;
  filter: drop-shadow(-1px 1px 0 rgb(255, 255, 255, 0.7)) drop-shadow(-1px -1px 0 rgb(255, 255, 255, 0.5));
}

.cartNumber {
  background-color: #098edb;
  border-radius: 20px;
  color: #e0e0e0;
  padding: 1px 7px 2px 7px;
}

.totalCart,
.cartLogo,
.cartNumber {
  cursor: pointer;
}

.total-cart {
  min-width: 100px;
}
</style>
