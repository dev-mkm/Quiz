<template>
  <v-app>
    <v-main class="main">
      <v-locale-provider rtl>
        <HelloWorld />
      </v-locale-provider>
    </v-main>
  </v-app>
</template>

<script setup>
  import HelloWorld from '@/components/HelloWorld.vue'
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Vazirmatn&display=swap');
.main {
  background: #0a1128;
  color: aliceblue;
}
* {
  font-family: 'Vazirmatn', sans-serif;
}
</style>