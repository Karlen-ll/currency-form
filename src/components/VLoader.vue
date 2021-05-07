<template>
  <div class="loader">
    <div class="spinner spinner--center">
      <div class="spinner__dot" />
      <div class="spinner__dot" />
      <div class="spinner__dot" />
    </div>
  </div>
</template>

<script>
export default {
  name: 'VLoader',
}
</script>

<style lang="scss" scoped>
.loader {
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: fade-out(#fff, 0.2);
  position: absolute;
  z-index: 12;
}

.spinner {
  width: 21px;
  height: 10px;
  display: block;
  position: relative;
  margin: 0 auto;
  z-index: 99;

  &--center {
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    position: absolute;
    margin: auto;
  }

  &__dot {
    width: 4px;
    height: 10px;
    float: left;
    position: relative;
    background: $primary-color;
    animation: spinner-col 1s infinite ease-in-out;
    margin: 0 1px;

    &:nth-child(2) { animation-delay: 160ms; }

    &:last-child { animation-delay: 320ms; }
  }
}



@-webkit-keyframes spinner-col {
  0%,
  80%,
  100% {
    -webkit-transform: scaleY(1.8);
    transform: scaleY(1.8);
  }

  40% {
    -webkit-transform: scaleY(1);
    transform: scaleY(1);
  }
}

@keyframes spinner-col {
  0%,
  80%,
  100% {
    -webkit-transform: scaleY(1.8);
    transform: scaleY(1.8);
  }

  40% {
    -webkit-transform: scaleY(1);
    transform: scaleY(1);
  }
}
</style>
