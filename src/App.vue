<template>
  <div id="app">
    <v-form />
  </div>
</template>

<script>
import VForm from "@/components/VForm";

export default {
  name: 'App',

  components: {
    VForm
  }
}
</script>

<style>
#app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
