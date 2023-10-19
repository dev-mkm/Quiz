<template>
  <v-app>
    <v-main class="main">
      <HelloWorld />
    </v-main>
  </v-app>
</template>

<script setup>
  import HelloWorld from '@/components/HelloWorld.vue'
</script>

<style scoped>
.main {
  background: #0a1128;
  color: aliceblue;
}
</style>