<template>
  <div class="backdrop" @click.self="closeModal">
    <div class="container modall rounded-4">
      <div class="container">
        <div class="d-flex justify-content-between align-items-center px-2">
          <div>
            <h2 class="fw-bold">{{ title }}</h2>
          </div>
          <span @click="closeModal" class="close-button scale-hover"><i class="bi bi-x-circle-fill fs-5 text-danger"></i></span>
        </div>
        <hr />
        <div class="container cartContainer overflow-auto">
          <div v-if="cart.length == 0" class="d-grid justify-content-center text-center">
            <img src="../assets/emptyCart.png" class="img-fluid mb-4" alt="emptyCartImg" />
            <h3 class="display-6">Empty Cart !</h3>
          </div>
          <div v-else v-for="product in cart" :key="product" class="row productCartContainer rounded-4 mb-2 bg-light shadow-sm">
            <div class="col-3 image">
              <img :src="product.image" class="img-fluid rounded-5" alt="product-img" />
            </div>
            <div class="col description">
              <h5>{{ product.productName }}</h5>
              <p class="display-6 fs-6 container bold">
                {{ product.price + " DT" }}
              </p>
              <div class="d-flex">
                <button class="btn btn-outline-danger h-25">
                  <i class="bi bi-dash-lg" aria-hidden="true"></i>
                </button>
                <input type="text" class="form-control mx-1" value="1" />
                <button class="btn btn-outline-success h-25">
                  <i class="bi bi-plus-lg" aria-hidden="true"></i>
                </button>
              </div>
            </div>
            <div class="col-3 deleteButton d-flex justify-content-end align-items-center">
              <span><i @click="deleteProduct(product)" class="bi bi-trash-fill fs-3 deleteIcon"></i></span>
            </div>
          </div>
        </div>
        <div class="container my-4">
          <h2 class="mb-0">Total: {{ totalCart }}</h2>
          <p class="display-6 fs-6 ms-1">Tax inclus: {{ tax }}</p>
        </div>
        <hr />
        <div class="container d-flex justify-content-around">
          <button v-if="cart.length == 0" @click="closeModal" class="btn btn-outline-primary">Start Shopping <i class="bi bi-arrow-90deg-right"></i></button>
          <div v-else class="btn-group">
            <button class="btn btn-outline-secondary" @click="closeModal"><i class="bi bi-cart-plus"></i><span> ContinueShopping</span></button>
            <button class="btn btn-outline-success">
              <i class="bi bi-credit-card-fill"></i><span> Checkout ({{ totalCart }})</span>
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "modal-item",
  props: {
    title: {
      type: String,
      require: true,
    },
    cart: {
      type: Array,
      require: true,
    },
    totalCart: {
      type: String,
      require: true,
    },
    tax: {
      type: String,
      require: true,
    },
  },
  data() {
    return {};
  },
  methods: {
    closeModal() {
      this.$emit("closeModal");
    },
    deleteProduct(productModal) {
      this.$emit("deleteProduct", productModal);
    },
  },
};
</script>

<style>
.modall {
  width: 80vw;
  padding: 20px;
  margin: 100px auto;
  background: #eeeeee;
  z-index: 9999;
}

@media (min-width: 600px) {
  .modall {
    width: 40vw;
  }
}

.modall p {
  cursor: pointer;
}

.backdrop {
  top: 0;
  position: fixed;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
}

.cartContainer {
  max-height: 400px;
}

.deleteButton,
.close-button {
  cursor: pointer;
}

.deleteIcon {
  color: gray;
}

.deleteIcon:hover {
  color: rgb(224, 74, 74);
}

.close-button {
  transition: transform 0.1s ease-in-out;
}

.scale-hover:hover {
  transform: scale(1.4);
}
</style>
