<template>
  <section>
    <ul>
      <li v-for="blog in data" :key="blog._path">
        <NuxtLink :to="blog._path">{{ blog.title }}</NuxtLink>
      </li>
    </ul>
  </section>
</template>

<script setup>
  const { data } = await useAsyncData('blog', () => queryContent('blog').only(['_path', 'title']).find())
  console.log(data)
</script>

<style scoped>

</style>