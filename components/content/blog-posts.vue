<template>
  <slot :posts="posts">
    <section class="not-prose font-mono">
    <div class="column text-gray-400 text-sm">
      <div>date</div>
      <div>title</div>
    </div>
    <ul>
      <li v-for="blog in posts" :key="blog._path">
        <NuxtLink :to="blog._path" class="column group hover:bg-gray-100 dark:hover:bg-gray-800">
          <div :class="{ 'text-gray-50 dark:text-gray-900 group-hover:text-gray-100 group-hover:text-gray-800': blog.publishedAt === 'noYear', 'text-gray-400 :dark:text-gray-500': blog.publishedAt !== 'noYear' }">{{ blog.publishedAt?.slice(0,4) }}</div>
          <div>{{ blog.title }}</div>
        </NuxtLink>
      </li>
    </ul>
  </section>
  </slot>
</template>

<script setup>
const props = defineProps({
  limit: {
    type: Number,
    default: null
  }
})
  const { data } = await useAsyncData(
  'blog-list',
  () => {
    const query = queryContent('/blog')
      .where({ _path: { $ne: '/blog' }})
      .only(['_path', 'title', 'publishedAt'])
      .sort({ publishedAt: -1})

      if(props.limit) {
        query.limit(props.limit)
      }
      return query.find()
  })

  const posts = computed(() => {
    const obj = []
    data.value.forEach((item, index) => {
      if (index === 0 || data.value[index-1].publishedAt.slice(0,4) !== data.value[index].publishedAt.slice(0,4)) {
        obj.push({
          _path: item._path,
          publishedAt: item.publishedAt,
          title: item.title,
        })
      } else {
        obj.push({
          _path: item._path,
          publishedAt: 'noYear',
          title: item.title,
        })
      }
    })
    return obj
  })
</script>

<style scoped>
  .column {
    @apply flex items-center space-x-8 py-2 border-b border-gray-200 dark:border-gray-700
  }
</style>