<template>
  <div class="productdetail">
    <HeaderShayna />
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
      <div class="container">
        <div class="row">
          <div class="col-lg-12">
            <div class="breadcrumb-text product-more">
              <router-link to="/">
                <i class="fa fa-home"></i> Home
              </router-link>
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
                  <img class="product-big-img" :src="gambar_default" alt />
                </div>
                <div class="product-thumbs" v-if="productsType.galleries.length > 0">
                  <carousel class="product-thumbs-track ps-slider" :dots="false" :nav="false">
                    <div
                      class="pt"
                      v-for="image in productsType.galleries"
                      :key="image.id"
                      @click="changeImage(image.photo)"
                      :class="image.photo == gambar_default ? 'active' : ''"
                    >
                      <img :src="image.photo" alt />
                    </div>
                  </carousel>
                </div>
              </div>
              <div class="col-lg-6">
                <div class="product-details text-left">
                  <div class="pd-title">
                    <span>{{ productsType.type }}</span>
                    <h3>{{ productsType.name }}</h3>
                  </div>
                  <div class="pd-desc">
                    <p>{{ productsType.created_at }}</p>
                    <p v-html="productsType.description"></p>
                    <h4>${{ productsType.price }}</h4>
                  </div>
                  <div class="quantity">
                    <router-link to="/cart">
                    <a
                      @click="saveKeranjang(productsType.id, productsType.name, productsType.price, productsType.galleries[0].photo)"
                      href="#"
                      class="primary-btn pd-cart"
                    >Add To Cart</a>
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
    <RelatedShayna />
    <FooterShayna />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from "@/components/HeaderShayna.vue";
import carousel from "vue-owl-carousel";
import RelatedShayna from "@/components/RelatedShayna.vue";
import FooterShayna from "@/components/FooterShayna.vue";
import axios from "axios";

export default {
  name: "ProductDetail",
  components: {
    HeaderShayna,
    carousel,
    RelatedShayna,
    FooterShayna
  },

  data() {
    return {
      gambar_default: "",
      id: this.$route.params.id,
      productsType: [],
      keranjangUser: []
    };
  },

  methods: {
    changeImage(urlImage) {
      this.gambar_default = urlImage;
    },

    setDataImage(data) {
      this.productsType = data;
      this.gambar_default = this.productsType.galleries[0].photo;
    },

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
      .get("http://shayna-backend.belajarkoding.com/api/products", {
        params: {
          id: this.id
        }
      })
      .then(res => this.setDataImage(res.data.data))
      .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.product-thumbs .pt {
  margin-right: 10px;
}
</style>