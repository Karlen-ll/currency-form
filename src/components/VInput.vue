<template>
  <div class="input__wrapper">
    <label class="input__label" :for="id">{{ caption }}</label>
    <input
        :id="id"
        class="input"
        v-bind="$attrs"
        v-on="getListeners"

        :value="data"
        @blur="onInput"

        @keydown.up="onInc"
        @keydown.down="onDec"

        @keypress="isNumber($event)"
    />

    <div class="input__style" />

    <label class="input__currency" :for="id">{{ currency }}</label>

    <div class="input__controls">
      <v-button class="input__button" icon="up" type="button" @click="onInc"/>
      <v-button class="input__button" icon="down" type="button" @click="onDec"/>
    </div>
  </div>
</template>

<script>
import VButton from "@/components/Button";

export default {
  name: 'VInput',

  inheritAttrs: false,

  components: {
    VButton,
  },

  model: {
    prop: 'value',
    event: 'input'
  },

  props: {
    value: {},

    id: {
      type: String,
      required: true,
    },

    caption: {
      type: String,
      required: true,
    },

    currency: {
      type: String,
      required: true,
    },
  },

  watch: {
    value: function(newValue) {
        this.data = this.checkNewValue(newValue);
    },
  },

  data() {
    return {
      data: this.value,

      stepMap: [1, 0.1, 0.01, 0.001, 0.0001, 0.00001, 0.000001, 0.0000001, 0.00000001],

      countOfSign: 1,
      maxCountOfSign: 8
    };
  },

  computed: {
    getListeners() {
      let listeners = this.$listeners;
      if (listeners.input) delete listeners.input;
      return listeners ? listeners : null;
    },

    step() {
      return this.stepMap[this.countOfSign]
    },
  },

  methods: {
    checkNewValue(value) {
      const str = value.toString().replace(/[,-]/g, ".");
      let result = Number.parseFloat(str);
      this.getCountOfSign(result);
      result = this.formatting(result)
      return result
    },

    onInc() {
      this.data = this.formatting(Number.parseFloat(this.data) + this.step);
      this.$emit('input', this.data);
    },

    onDec() {
      this.data = this.formatting(Number.parseFloat(this.data) - this.step);
      this.$emit('input', this.data);
    },

    onInput(value) {
      this.$emit('input', value.target.value);
    },

    formatting(value) {
      const text = value.toString();
      const position = text.indexOf('.');
      if (position > 0) {
        const chunk = text.length - position;
        const max = position + (chunk > 7 ? 8 : chunk) + 1
        return text.toString().substring(0, position + 1) + text.toString().substring(position + 1, max)
      }
      return value
    },

    isNumber: function(event) {
      event = (event) ? event : window.event;
      const isNotFloat = event.target.value.indexOf('.') === -1;

      let charCode = (event.which) ? event.which : event.keyCode;
      if ((charCode > 47 && charCode < 58) || (charCode === 46 && isNotFloat)) return true
      if (charCode === 44 && isNotFloat) {
        const position = event.target.selectionStart
        const text = event.target.value.toString();
        if (position > 0) event.target.value = text.substring(0, position) + '.' + text.substring(position, text.length)
      }
      event.preventDefault();
    },

    getCountOfSign(num) {
      let i = 0;
      if (num % 1 !== 0){
        do {
          num = num * 10;
          i++;
        } while (num % 1 !== 0);
      }
      this.countOfSign = i < this.maxCountOfSign ? i : this.maxCountOfSign;
    }
  },
}
</script>

<style lang="scss" scoped>
.input {
  $name: input;

  flex: 1 0;
  border: 0;
  text-align: right;
  background-color: transparent;
  color: #222222;
  margin: 0 0.5em;
  z-index: 10;

  &:focus {
    outline: none;

    ~ .#{$name}__style {
      -webkit-box-shadow: 0 0 4px 0 fade-out($primary-color, 0.5);
      -moz-box-shadow: 0 0 4px 0 fade-out($primary-color, 0.5);
      box-shadow: 0 0 4px 0 fade-out($primary-color, 0.5);
      border-color: $primary-color;
    }
  }

  &__wrapper {
    display: flex;
    position: relative;
    align-items: center;

    padding: 0 0 0 0.7em;
    margin: 1em 0;

    &:first-child { margin-top: 0; }
    &:last-child { margin-bottom: 0; }

    &:hover {
      .#{$name}__style { border-color: $primary-color; }
    }
  }

  &__style {
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    transition: border $duration;
    border-radius: 0.3em 0 0 0.3em;
    border: 1px solid $border-color;
    position: absolute;
    z-index: 1;
  }

  &__controls {
    display: flex;
    flex-direction: column;
    z-index: 5;

    .button {
      display: flex;
      border-width: 1px 0 0 1px;
      align-items: center;
      justify-content: center;

      &:first-child {
        border-top-width: 0;
        margin-top: 1px;
      }

      &:last-child { margin-bottom: -1px; }
    }
  }

  &__label,
  &__currency {
    color: $grey-color;
    z-index: 2;
  }

  &__currency {
    margin-right: 0.65em;
  }
}
</style>