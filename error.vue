<template>
  <NuxtLayout>
    <h3 class="text-2xl mb-4">Oops you found it! - {{ error.message }}</h3>
    <p>Go back to <NuxtLink to="/" class="text-green-600 dark:text-green-400">safety</NuxtLink> :-) </p>
  </NuxtLayout>
</template>

<script setup>
  defineProps({ error: Object})
</script>

<style scoped>

</style>