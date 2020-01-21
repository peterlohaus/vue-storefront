<template>
  <div class="header">
    <header class="fixed w-100" :class="{ 'is-visible': navVisible }">
      <div class="container">
        <nav class="row" aria-label="account navigation">
          <PhoneIcon class="col-lg-2 center-lg middle-lg" />
          <MailIcon class="col-lg-2 center-lg middle-lg" />
          <AccountIcon class="col-lg-offset-5 col-lg-1 center-lg middle-lg" />
          <MicrocartIcon class="col-lg-1 center-lg middle-lg" />
          <LanguageIcon class="col-lg-1 center-lg middle-lg" />
        </nav>
        <nav class="row" aria-label="catalog navigation">
          <SearchIcon class="col-lg-1 center-lg middle-lg" />
          <CompanyIcon class="col-lg-offset-2 col-lg-6 center-lg middle-lg" />
          <HamburgerIcon class="col-lg-offset-2 col-lg-1 center-lg middle-lg" />
        </nav>
      </div>
    </header>
  </div>
  <!--     
      <div class="header">
      <header
      class="fixed w-100 brdr-bottom-1 bg-cl-primary brdr-cl-secondary"
      :class="{ 'is-visible': navVisible }"
    >
      <div class="container px15">
        <div class="row between-xs middle-xs" v-if="!isCheckoutPage || isThankYouPage">
          <div class="col-md-4 col-xs-2 middle-xs">
            <div>
              <hamburger-icon class="p15 icon bg-cl-secondary pointer" />
            </div>
          </div>
          <div class="col-xs-2 visible-xs">
            <search-icon class="p15 icon pointer" />
          </div>
          <div class="col-md-4 col-xs-4 center-xs pt5">
            <div>
              <logo width="auto" height="41px" />
            </div>
          </div>
          <div class="col-xs-2 visible-xs">
            <wishlist-icon class="p15 icon pointer" />
          </div>
          <div class="col-md-4 col-xs-2 end-xs">
            <div class="inline-flex right-icons">
              <search-icon class="p15 icon hidden-xs pointer" />
              <wishlist-icon class="p15 icon hidden-xs pointer" />
              <compare-icon class="p15 icon hidden-xs pointer" />
              <microcart-icon class="p15 icon pointer" />
              <account-icon class="p15 icon hidden-xs pointer" />
            </div>
          </div>
        </div>
        <div class="row between-xs middle-xs px15 py5" v-if="isCheckoutPage && !isThankYouPage">
          <div class="col-xs-5 col-md-3 middle-xs">
            <div>
              <router-link
                :to="localizedRoute('/')"
                class="cl-tertiary links"
              >
                {{ $t('Return to shopping') }}
              </router-link>
            </div>
          </div>
          <div class="col-xs-2 col-md-6 center-xs">
            <logo width="auto" height="41px" />
          </div>
          <div class="col-xs-5 col-md-3 end-xs">
            <div>
              <a
                v-if="!currentUser"
                href="#"
                @click.prevent="gotoAccount"
                class="cl-tertiary links"
              >{{ $t('Login to your account') }}</a>
              <span v-else>{{ $t('You are logged in as {firstname}', currentUser) }}</span>
            </div>
          </div>
        </div>
      </div>
    </header>
    <div class="header-placeholder" />
  </div>-->
</template>

<script>
import { mapState } from "vuex";
import CurrentPage from "theme/mixins/currentPage";
import AccountIcon from "theme/components/core/blocks/Header/AccountIcon";
import CompareIcon from "theme/components/core/blocks/Header/CompareIcon";
import HamburgerIcon from "theme/components/core/blocks/Header/HamburgerIcon";
import Logo from "theme/components/core/Logo";
import MicrocartIcon from "theme/components/core/blocks/Header/MicrocartIcon";
import SearchIcon from "theme/components/core/blocks/Header/SearchIcon";
import WishlistIcon from "theme/components/core/blocks/Header/WishlistIcon";

export default {
  name: "Header",
  components: {
    AccountIcon,
    CompareIcon,
    HamburgerIcon,
    Logo,
    MicrocartIcon,
    SearchIcon,
    WishlistIcon
  },
  mixins: [CurrentPage],
  data() {
    return {
      navVisible: true,
      isScrolling: false,
      scrollTop: 0,
      lastScrollTop: 0,
      navbarHeight: 54
    };
  },
  computed: {
    ...mapState({
      isOpenLogin: state => state.ui.signUp,
      currentUser: state => state.user.current
    }),
    isThankYouPage() {
      return this.$store.state.checkout.isThankYouPage
        ? this.$store.state.checkout.isThankYouPage
        : false;
    }
  },
  beforeMount() {
    window.addEventListener(
      "scroll",
      () => {
        this.isScrolling = true;
      },
      { passive: true }
    );

    setInterval(() => {
      if (this.isScrolling) {
        this.hasScrolled();
        this.isScrolling = false;
      }
    }, 250);
  },
  methods: {
    gotoAccount() {
      this.$bus.$emit("modal-toggle", "modal-signup");
    },
    hasScrolled() {
      this.scrollTop = window.scrollY;
      if (
        this.scrollTop > this.lastScrollTop &&
        this.scrollTop > this.navbarHeight
      ) {
        this.navVisible = false;
      } else {
        this.navVisible = true;
      }
      this.lastScrollTop = this.scrollTop;
    }
  }
};
</script>

<style lang="scss" scoped>
@import "~theme/css/variables/colors";
@import "~theme/css/helpers/functions/color";
$color-icon-hover: color(secondary, $colors-background);

* {
  border: 1px solid red;
}

header {
  height: 54px;
  top: -55px;
  z-index: 3;
  transition: top 0.2s ease-in-out;
  &.is-visible {
    top: 0;
  }
}
.icon {
  opacity: 0.6;
  &:hover,
  &:focus {
    background-color: $color-icon-hover;
    opacity: 1;
  }
}
.right-icons {
  //for edge
  float: right;
}
.header-placeholder {
  height: 54px;
}
.links {
  text-decoration: underline;
}
@media (max-width: 767px) {
  .row.middle-xs {
    margin: 0 -15px;

    &.py5 {
      margin: 0;
    }
  }
  .col-xs-2:first-of-type {
    padding-left: 0;
  }
  .col-xs-2:last-of-type {
    padding-right: 0;
  }
  a,
  span {
    font-size: 12px;
  }
}
</style>
