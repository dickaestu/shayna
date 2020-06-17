<template>
  <!-- Women Banner Section Begin -->
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <carousel class="product-slider" :dots="false" :items="3" :nav="false" :autoplay="true">
            <div class="product-item" v-for="itemProduct in products" :key="itemProduct.id">
              <div class="pi-pic">
                <img v-bind:src="itemProduct.galleries[0].photo" alt />
                <ul>
                  <li class="w-icon active">
                    <a
                      @click="saveCart(itemProduct.id, itemProduct.name, itemProduct.price, itemProduct.galleries[0].photo)"
                      href="#"
                    >
                      <i class="icon_bag_alt"></i>
                    </a>
                  </li>
                  <li class="quick-view">
                    <router-link v-bind:to="'/product/'+itemProduct.id">+ Quick View</router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ itemProduct.type }}</div>
                <router-link v-bind:to="'/product/'+itemProduct.id">
                  <h5>{{ itemProduct.name }}</h5>
                </router-link>

                <div class="product-price">IDR. {{ itemProduct.price }}</div>
              </div>
            </div>
          </carousel>
        </div>

        <div class="col-lg-12" v-else>
          <p>Produk terbaru belum tersedia untuk saat ini</p>
        </div>
      </div>
    </div>
  </section>
  <!-- Women Banner Section End -->
</template>

<script>
import carousel from "vue-owl-carousel";
import axios from "axios";

export default {
  name: "BannerShayna",
  components: {
    carousel
  },
  data() {
    return {
      products: [],
      cartUser: []
    };
  },
  methods: {
    saveCart(idProduct, nameProduct, priceProduct, photoProduct) {
      var productStored = {
        id: idProduct,
        name: nameProduct,
        price: priceProduct,
        photo: photoProduct
      };

      this.cartUser.push(productStored);
      const parsed = JSON.stringify(this.cartUser);
      localStorage.setItem("cartUser", parsed);

      window.location.reload();
    }
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
      .get("http://backend-shayna.dickaestu.my.id/api/products")
      .then(res => (this.products = res.data.data.data))
      // eslint-disable-next-line no-console
      .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.product-item {
  margin-right: 25px;
}
</style>