<template>
  <router-link :to="localizedRoute('/')" :title="$t('Home Page')" class="no-underline inline-flex">
    <div class="logo-ballondrucker logo-ballondrucker-extended" />
  </router-link>
</template>

<script>
import config from "config";
import { currentStoreView } from "@vue-storefront/core/lib/multistore";

export default {
  data() {
    const storeView = currentStoreView();
    return {
      defaultTitle: storeView.seo.defaultTitle
        ? storeView.seo.defaultTitle
        : config.seo.defaultTitle
    };
  }
};
</script>

<style lang="scss" scoped>

.logo-ballondrucker {
  position: absolute;
  background-image: url(/assets/logo_ballondrucker.png);
  background-size: cover;
  width: 100%;
  height: 0;
  padding: 0;
  padding-bottom: calc(100% * 116 / 492);
  margin-bottom: 15px;
  transform: scale(0.4);
  transition: all .4s;
}

.is-visible .logo-ballondrucker-extended {
  margin-bottom: 70px;
  transform: scale(0.6);
  transition: all .4s .1s;
}

</style>
