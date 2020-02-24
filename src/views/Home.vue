<template>
  <div class="home">
    <HeaderShayna/>
    <HeroShayna/>
    <WomenShayna/>
    <InstagramShayna/>
    <PartnerLogoShayna/>
    <FooterShayna/>
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from "@/components/HeaderShayna.vue";
import HeroShayna from "@/components/HeroShayna.vue";
import WomenShayna from "@/components/WomenShayna.vue";
import InstagramShayna from "@/components/InstagramShayna.vue";
import PartnerLogoShayna from "@/components/PartnerLogoShayna.vue";
import FooterShayna from "@/components/FooterShayna.vue";

export default {
  name: "Home",
  components: {
    HeaderShayna,
    HeroShayna,
    WomenShayna,
    InstagramShayna,
    PartnerLogoShayna,
    FooterShayna
  }
};
</script>
