<template>
  <div class="produt">
    <Header />

    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
              <span>Detail</span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Breadcrumb Section Begin -->
    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="row">
              <div class="col-lg-6">
                <div class="product-pic-zoom">
                  <img class="product-big-img" :src="gambar_default" alt="" />
                </div>
                <div
                  class="product-thumbs"
                  v-if="productDetails.gallery.length > 0"
                >
                  <carousel
                    :dots="false"
                    :nav="false"
                    class="product-thumbs-track ps-slider"
                  >
                    <div
                      v-for="detail in productDetails.gallery"
                      :key="detail.id"
                      class="pt"
                      :class="detail.photo == gambar_default ? 'active' : ''"
                      @click="changeImage(detail.photo)"
                    >
                      <img :src="detail.photo" alt="" />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productDetails.type }}</span>
                    <h3>{{ productDetails.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p v-html="productDetails.description"></p>
                    <h4>{{ productDetails.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart" class="primary-btn pd-cart">
                      <a
                        @click="
                          storeCart(
                            productDetails.id,
                            productDetails.name,
                            productDetails.price,
                            productDetails.gallery[0].photo
                          )
                        "
                        href="#"
                        class="primary-btn pd-cart"
                        >Add To Cart</a
                      >
                    </router-link>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- Product Shop Section End -->

    <RelatedProduct />
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import Footer from "@/components/Footer.vue";
import RelatedProduct from "@/components/RelatedProduct.vue";
import carousel from "vue-owl-carousel";
import axios from "axios";
export default {
  name: "Product",
  components: {
    Header,
    Footer,
    carousel,
    RelatedProduct,
  },
  data() {
    return {
      gambar_default: "",
      //idProduct: this.$route.params.id,
      productDetails: [],
      cartUser: [],
    };
  },
  methods: {
    changeImage(urlImage) {
      this.gambar_default = urlImage;
      //console.log(this.idProduct);
    },
    setImageResource(data) {
      this.productDetails = data;
      this.gambar_default = data.gallery[0].photo;
    },
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
    },
  },
  mounted() {
    if (localStorage.getItem("cartUser")) {
      try {
        this.cartUser = JSON.parse(localStorage.getItem("cartUser"));
      } catch (e) {
        localStorage.removeItem("cartUser");
      }
    }
    axios
      .get("http://localhost/backend-shop/public/api/products", {
        params: {
          id: this.$route.params.id,
        },
      })
      .then((res) => this.setImageResource(res.data.data))
      .catch((err) => console.log(err));
  },
};
</script>
<style scoped>
.product-thumbs .pt {
  margin-right: 12px;
}
</style>
