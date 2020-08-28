<template>
  <div class="home">
    <Header />
    <CarouselSlider />
    <ListProduct />
    <Instagram />
    <Partner />
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import CarouselSlider from "@/components/CarouselSlider.vue";
import ListProduct from "@/components/ListProduct.vue";
import Instagram from "@/components/Instagram.vue";
import Partner from "@/components/Partner.vue";
import Footer from "@/components/Footer.vue";

export default {
  name: "Home",
  components: {
    Header,
    CarouselSlider,
    ListProduct,
    Instagram,
    Partner,
    Footer,
  },
};
</script>
