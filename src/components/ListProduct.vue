<template>
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <carousel
            class="product-slider"
            :items="3"
            :nav="false"
            :autoplay="true"
            :dots="false"
          >
            <div
              class="product-item"
              v-for="product in products"
              :key="product.id"
            >
              <div class="pi-pic">
                <img :src="product.gallery[0].photo" alt="" />
                <ul>
                  <li class="w-icon active">
                    <a
                      href="#"
                      @click="
                        storeCart(
                          product.id,
                          product.name,
                          product.price,
                          product.gallery[0].photo
                        )
                      "
                      ><i class="icon_bag_alt"></i
                    ></a>
                  </li>
                  <li class="quick-view">
                    <router-link to="/product">+ Quick View</router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ product.type }}</div>
                <router-link v-bind:to="'/product/' + product.id">
                  <h5>{{ product.name }}</h5>
                </router-link>
                <div class="product-price">
                  {{ product.price }}
                  <span>$35.00</span>
                </div>
              </div>
            </div>
          </carousel>
        </div>
        <div class="col-lg-12" v-else>
          <p>
            Produk Baru Tidak Ditemukan
          </p>
        </div>
      </div>
    </div>
  </section>
</template>
<script>
import carousel from "vue-owl-carousel";
import axios from "axios";
export default {
  name: "ListProduct",
  components: {
    carousel,
  },
  data() {
    return {
      products: [],
      cartUser: [],
    };
  },
  methods: {
    storeCart(idProduct, name, price, photo) {
      var productStored = {
        id: idProduct,
        name: name,
        price: price,
        photo: photo,
      };
      this.cartUser.push(productStored);
      const parsed = JSON.stringify(this.cartUser);
      localStorage.setItem("cartUser", parsed);
      window.location.reload();
    },
  },
  mounted() {
    axios
      .get("http://localhost/backend-shop/public/api/products")
      .then((res) => (this.products = res.data.data.data))
      .catch((err) => console.log(err));
    if (localStorage.getItem("cartUser")) {
      try {
        this.cartUser = JSON.parse(localStorage.getItem("cartUser"));
      } catch (e) {
        localStorage.removeItem("cartUser");
      }
    }
  },
};
</script>
<style scoped>
.product-item {
  margin-right: 25px;
}
</style>
