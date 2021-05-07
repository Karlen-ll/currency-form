<template>
  <form @submit="onSubmit" class="form">
    <transition name="loading">
      <v-loader v-if="loading"/>
    </transition>

    <header class="header">
      <div class="header__logo">
        <MainIcon/>
      </div>

      <div class="header__status">
        <span class="header__bye"><span class="value">{{ bye }}</span> KICK</span>
        <span class="header__sell"><span class="value">{{ sell }}</span> ETH</span>
      </div>
    </header>

    <main class="main">
      <v-input id="price" caption="Price" currency="ETH" v-model="price"/>
      <v-input id="amount" caption="Amount" currency="KICK" v-model="amount"/>
      <v-input id="total" caption="Total ~" currency="ETH" v-model="total"/>
    </main>

    <footer class="footer">
      <v-button
          class="footer__button button--bye"
          :disabled="loading"
          type="submit"
          uppercase
          @click.prevent="onBye"
      >
        Buy
      </v-button>
      <v-button
          class="footer__button button--sell"
          :disabled="loading"
          type="submit"
          uppercase
          @click.prevent="onSell"
      >
        Sell
      </v-button>
    </footer>
  </form>
</template>

<script>
import { MainIcon } from "@/components/Icon";
import VButton from "@/components/Button";
import VInput from "@/components/VInput";
import VLoader from "@/components/VLoader";

export default {
  name: 'VForm',

  components: {
    MainIcon,
    VLoader,
    VButton,
    VInput,
  },

  watch: {
    price: function (newValue) {
      if (!isNaN(newValue)) this.total = this.amount * newValue
    },

    amount: function (newValue) {
      if (!isNaN(newValue)) this.total = newValue * this.price
    },

    total: function (newValue) {
      if (!isNaN(newValue)) this.amount = this.price > 0 ? newValue / this.price : 0
    },
  },

  data() {
    return {
      bye: 0,
      sell: 0,

      loading: false,

      price: 10000,
      amount: 0,
      total: 0,
    }
  },

  methods: {
    onBye() {
      if (this.total > 0) {
        this.onSubmit();

        setTimeout(() => {
          this.bye += this.total;
        }, 1250)
      }
    },

    onSell() {
      if (this.total > 0) {
        this.onSubmit();

        setTimeout(() => {
          this.sell += this.total;
        }, 1250)
      }
    },

    onSubmit() {
      this.loading = true;

      setTimeout(() => {
        this.loading = false;
      }, 1000)
    },
  },
}
</script>

<style lang="scss" scoped>
$bye-color: #13ab13;
$sell-color: #b72626;

.form {
  position: relative;
}

.header {
  display: flex;

  &__logo {
    margin: 0 1em 0 0.7em;
  }

  &__status {
    display: flex;
    flex-direction: column;
  }

  &__bye {
    color: $bye-color;
  }

  &__sell {
    color: $sell-color;
  }
}

.main {
  margin: 1em 0 1.5em;
}

.footer {
  display: flex;

  .button {
    flex: 1 0;
    margin-right: 1em;
    text-align: center;
    border-radius: 0.3em;
    padding: 0.5em;

    &--bye {
      color: white;
      background-color: $bye-color;

      &:hover,
      &:focus {
        -webkit-box-shadow: 0 0 6px 0 fade-out($bye-color, 0.5);
        -moz-box-shadow: 0 0 6px 0 fade-out($bye-color, 0.5);
        box-shadow: 0 0 6px 0 fade-out($bye-color, 0.5);
      }
    }

    &--sell {
      color: $sell-color;
      border-color: $sell-color;

      &:hover,
      &:focus {
        -webkit-box-shadow: 0 0 4px 0 fade-out($sell-color, 0.5);
        -moz-box-shadow: 0 0 4px 0 fade-out($sell-color, 0.5);
        box-shadow: 0 0 4px 0 fade-out($sell-color, 0.5);
      }
    }

    &:last-child {
      margin-right: 0;
    }
  }
}
</style>
