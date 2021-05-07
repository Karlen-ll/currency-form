<template>
  <svg class="icon" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" role="presentation">
    <title>{{ glyph }} icon</title>

    <use :href="`#icon_${ glyph }`" />
  </svg>
</template>

<script>
export default {
  name: 'VIcon',

  props: {
    glyph: {
      type: String,
      default: 'up',
      validator: value => {
        return ['up', 'down',].includes(value);
      },
    },
  },
};
</script>

<style lang="scss" scoped>
.icon {
  fill: currentColor;
  transition: fill 0.35s linear;
  width: 1em;
  height: 1em;
}
</style>
