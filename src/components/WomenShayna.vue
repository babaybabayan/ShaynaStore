<template>
  <!-- Women Banner Section Begin -->
  <section class="women-banner spad">
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-12 mt-5" v-if="products.length > 0">
          <carousel class="product-slider" :nav='false' :dots="false">
            <div class="product-item" v-for="items in products" :key="items.id">
              <div class="pi-pic">
                <img :src="items.galleries[0].photo" alt />
                <ul>
                  <li class="w-icon active">
                    <a @click="saveKeranjang(items.id, items.name, items.price, items.galleries[0].photo)" href="#">
                      <i class="icon_bag_alt"></i>
                    </a>
                  </li>
                  <li class="quick-view">
                    <router-link :to="'/product/'+items.id">
                    Quick View
                    </router-link>
                  </li>
                </ul>
              </div>
              <div class="pi-text">
                <div class="catagory-name">{{ items.type }}</div>
                <a href="#">
                  <h5>{{ items.name }}</h5>
                </a>
                <div class="product-price">
                  ${{ items.price }}
                  <span>$35.00</span>
                </div>
              </div>
            </div>
          </carousel>
        </div>
        <div class="col-lg-12" v-else>
          <p>Data Tidak Tersedia</p>
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
  name: "WomenShayna",
  components: {
    carousel
  },

  data() {
    return {
      products: [],
      keranjangUser: []
    }
  },

  methods: {
    saveKeranjang(id, name, price, photo) {
      
      var productStored = {
        "id": id,
        "name": name,
        "price": price,
        "photo": photo
      }

      this.keranjangUser.push(productStored);
      const parsed = JSON.stringify(this.keranjangUser);
      localStorage.setItem("keranjangUser", parsed);
    }
  },

  mounted() {
    if (localStorage.getItem("keranjangUser")) {
      try {
        this.keranjangUser = JSON.parse(localStorage.getItem("keranjangUser"));
      } catch (e) {
        localStorage.removeItem("keranjangUser");
      }
    }

    axios
    .get("http://shayna-backend.belajarkoding.com/api/products")
    .then(res =>  (this.products = res.data.data.data))
    .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.product-item {
    margin-right: 10px;
}

.product-item .pi-pic img {
    height: 450px;
}

</style>