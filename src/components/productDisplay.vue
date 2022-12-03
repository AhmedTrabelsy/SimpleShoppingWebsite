<template>
  <div class="product-display">
    <div class="product-container">
      <div class="product-image position-relative">
        <img v-if="false" src="../assets/out-of-stock-img.png" class="w-25 out-of-stock-img position-absolute"
          alt="out-of-stock-img" />
        <img class="rounded-4 shadow product-img-src" :src="image" alt="Product"
          :class="{ disabledButton: inStock == false }" />
      </div>
      <div class="product-info">
        <h1 class="display-1">{{ currentVariant.productName }}</h1>
        <p>{{ currentVariant.description }}</p>
        <div class="d-flex justify-content-between pl-5 pr-5">
          <div class="d-flex">
            <div>
              <h3 class="font-weight-bold">{{ price }}</h3>
            </div>
            <div class="">
              <h3 class="display-6 text-decoration-line-through ms-3 fs-6 text-secondary">{{ oldprice }}</h3>
            </div>
          </div>
          <div class="Stock">
            <div v-if="inStock">
              <h4 :style="{ color: stockColor }">{{ stock }}</h4>
            </div>
            <div v-else>
              <h4 :style="{ color: stockColor }">Repture de stock</h4>
            </div>
            <div class="progress border border-secondary">
              <div class="progress-bar" :style="{ 'width': progresStock, 'background-color': stockColor }">{{
                  progresStock
              }}</div>
            </div>
          </div>
        </div>
        <hr />
        <div class="row d-flex justify-content-between">
          <div class="col-6 d-flex align-items-center shippingContainer">
            <h6 v-if="shipping"><i class="bi bi-truck fs-5"></i> Free Shipping !</h6>
            <h6 v-else><i class="bi bi-truck fs-5"></i> Shipping fees: 7dt only<p class='display-6 fs-6'>Free shipping
                for primium users over 300 Dt </p>
            </h6>
          </div>
          <div class="ratingContainer col-3 d-flex flex-column align-items-center me-3">
            <span>{{ "(" + stars + "/5)" }}</span>
            <div class="starsContainer d-flex align-items-center">
              <span v-for="index in 5" :key="index" class="starContainer">
                <i class="bi ms-1 text-warning" :class="{ 'bi-star': stars < index, 'bi-star-fill': true }"></i>
              </span>
            </div>
          </div>
        </div>
        <hr />
        <h3 class="font-weight-bold">Details</h3>
        <div class="container">
          <div class="row">
            <div class="col-6 col-xs-10">
              <productdetails :details="currentVariant.details" />
            </div>
            <div class="col col-xs-1">
              <div :style="{ backgroundColor: variant.color }" class="colorField"
                :class="{ 'selected': index == variantIndex }" v-for="(variant, index) in variants" :key="variant.id"
                @click="changeVariant(index)"></div>
            </div>
            <div class="col col-xs-1">
              <div class="row">
                <label class="font-weight-bold">Pointures</label>
              </div>
              <div class="row">
                <select class="form-select">
                  <option v-for="size in sizes" :key="size.id">{{ size }}</option>
                </select>
              </div>
            </div>
          </div>
          <div class="d-flex justify-content-center btn-group mt-4">
            <button class="btn btn-outline-danger btn-lg" @click="removeCart">
              Remove From cart
            </button>
            <button class="btn btn-outline-success btn-lg" @click="addCart" :disabled="!inStock"><i
                class="bi bi-cart-plus-fill"></i>
              Add to cart
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import productdetails from "../components/productDetails.vue";

export default {
  name: "productDisplay",
  components: {
    productdetails,
  },
  props: {
    cart: {
      type: Array,
      required: true,
    },
    primium: {
      type: Boolean,
      required: true,
    },
    totalCart: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      variants: [
        {
          id: 2001,
          productName: "Monsef - Vert",
          description:
            "Kalb w ji3an w y5abech barcha",
          details: ["50% coton", "30% laine", "20% polyester"],
          ratings: [{ stars: 5 }, { stars: 4 }],
          oldPrice: 78,
          price: 45,
          color: "Green",
          image: require("../assets/socks_green.jpg"),
          qty: 95,
          lastStock: 120
        },
        {
          id: 2002,
          productName: "Monsef - Bleu",
          description:
            "Lorem ipsum dolor sit amet consectetur adipisicing elit. Totam earum fugit voluptate, quasi beatae quaerat quo natus iusto voluptates, incidunt molestias obcaecati. Molestias temporibus aperiam pariatur minima animi repudiandae deserunt possimus repellat voluptates illum labore quae quod velit, minus assumenda voluptatum quisquam ea nostrum.",
          details: ["50% coton", "30% laine", "20% polyester"],
          ratings: [],
          oldPrice: 94,
          price: 50,
          color: "Blue",
          image: require("./../assets/socks_blue.jpg"),
          qty: 5,
          lastStock: 30
        },
      ],
      sizes: ["22-13", "15-64", "19-94"],
      variantIndex: 0,
    };
  },
  methods: {
    changeVariant(index) {
      this.variantIndex = index;
    },
    addCart() {
      if (this.inStock) {
        this.currentVariant.qty--;
        this.$emit("add-to-cart", this.currentVariant);
      }
    },
    removeCart() {
      if (this.cart != undefined && this.cart.indexOf(this.currentVariant) >= 0) {
        this.currentVariant.qty++;
        this.$emit("remove-from-cart", this.currentVariant);
      }
    },
  },
  computed: {
    currentVariant() {
      return this.variants[this.variantIndex];
    },
    image() {
      return this.currentVariant.image;
    },
    qty() {
      return this.currentVariant.qty;
    },
    stock() {
      if (this.qty == 1) {
        return "Dernier article disponible";
      } else if (this.qty <= 10) {
        return this.qty + " articles seulement";
      }
      return this.qty + " articles restants";
    },
    stockColor() {
      if (this.qty == 1) {
        return "#F7DA00";
      } else if (this.qty == 0) {
        return "#E0144C";
      } else if (this.qty <= 10) {
        return "#E48900";
      }
      return "#82CD47";
    },
    inStock() {
      if (this.qty == 0) {
        return false;
      }
      return true;
    },
    price() {
      return this.currentVariant.price + " TTC";
    },
    oldprice() {
      return this.currentVariant.oldPrice + " TTC";
    },
    shipping() {
      if (this.primium && this.totalCart > 300) {
        return true;
      }
      return false;
    },
    progresStock() {
      return parseInt(100 / (this.currentVariant.lastStock / this.currentVariant.qty)) + '%';
    },
    progresStockWidth() {
      return 'width: ' + this.progresStock + '%;';
    },
    stars() {
      let count = 0;
      this.currentVariant.ratings.forEach(element => {
        count += element.stars;
      });
      if (this.currentVariant.ratings.length == 0) {
        return 0;
      }
      return count / this.currentVariant.ratings.length;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.button {
  margin: 30px;
  background-color: #39495c;
  border-radius: 5px;
  font-size: 18px;
  width: 160px;
  height: 60px;
  color: white;
  padding: 20px;
  box-shadow: inset 0 -0.6em 1em -0.35em rgba(0, 0, 0, 0.17),
    inset 0 0.6em 2em -0.3em rgba(255, 255, 255, 0.15),
    inset 0 0 0em 0.05em rgba(255, 255, 255, 0.12);
  text-align: center;
  cursor: pointer;
}

.disabledButton {
  background-color: #d8d8d8;
  cursor: not-allowed;
  filter: grayscale(100) blur(5px);
}

.product-info h1 {
  font-size: 40px;
  color: #098EDB;
}

h3 {
  font-size: 25px;
}

.product-image .product-img-src {
  border: 2px solid #d8d8d8;
  width: 70%;
  margin: 40px;
  padding: 15px;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 15px -12px rgba(0, 0, 0, 0.57);
}

.out-of-stock-img {
  top: 135px;
  left: 145px;
}

input {
  width: 100%;
  height: 40px;
  margin-bottom: 20px;
}

label {
  font-size: 20px;
  margin-bottom: 5px;
}

li {
  font-size: 15px;
}

p {
  font-size: 16px;
}

.product-display {
  display: flex;
  flex-direction: column;
  padding: 2.5rem 1rem 1rem 1rem;
}

.product-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.product-image {
  width: 45%;
}

.product-info {
  width: 50%;
}

.review-form {
  display: flex;
  flex-direction: column;
  width: 425px;
  padding: 20px;
  margin: 40px;
  border: 2px solid #d8d8d8;
  background-color: white;
  -webkit-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 15px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 15px -12px rgba(0, 0, 0, 0.57);
}

.review-container {
  width: 425px;
  padding: 20px;
  background-color: white;
  -webkit-box-shadow: 0px 2px 20px -12px rgba(0, 0, 0, 0.57);
  -moz-box-shadow: 0px 2px 20px -12px rgba(0, 0, 0, 0.57);
  box-shadow: 2px 20px -12px rgba(0, 0, 0, 0.57);
  margin-left: 40px;
  border: 2px solid #d8d8d8;
}

.review-container li {
  margin-bottom: 30px;
}

.review-form .button {
  display: block;
  margin: 30px auto;
}

select {
  height: 40px;
  font-size: 20px;
  background-color: white;
  cursor: pointer;
}

textarea {
  width: 95%;
  height: 70px;
  padding: 10px;
  font-size: 20px;
  margin-bottom: 20px;
}

ul {
  list-style-type: none;
}

@media only screen and (max-width: 600px) {
  .container {
    flex-direction: column;
  }

  .product-image,
  .product-info {
    margin-left: 10px;
    width: 100%;
  }

  .review-form {
    width: 90%;
  }

  .total-cart {
    min-width: 70px;
  }
}

.selected {
  outline: dashed rgba(57, 73, 92, 0.7);
}

.colorField {
  margin: 5px;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  outline-offset: 1px;
  border: 2px solid #d8d8d8;
  cursor: pointer;
  transition: transform 0.2 ease-in-out;
}

.colorField:hover {
  transform: scale(1.1);
}
</style>
