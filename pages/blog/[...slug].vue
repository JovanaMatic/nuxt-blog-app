<template>
    <article class="prose dark:prose-invert prose-pre:bg-white dark:prose-pre:bg-gray-800 prose-pre:text-gray-700 dark:prose-pre:text-gray-300 max-w-none">

      <ContentDoc>
      <template #not-found>
        <h1>Oooppppssss not found</h1>
      </template>
      <template v-slot="{ doc }">
        <div class="grid grid-cols-6 gap-16">
          <div :class="{ 'col-span-6 md:col-span-4' : doc.toc, 'col-span-6' : !doc.toc }">
            <ContentRenderer :value="doc" />
          </div>
          <div class="hidden md:col-span-2 md:block col-span-2 not-prose" v-if="doc.toc">
            <aside class="sticky top-8">
              <div class="font-semibold mb-2">
                Table of Contents
              </div>
              <nav>
                <TocLinks :links="doc.body.toc.links" :active-Id="activeId" />
              </nav>
            </aside>
          </div>
        </div>
        </template>
      </ContentDoc>
    </article>
</template>

<script setup>
  const activeId = ref(null)

  onMounted(() => {
    let elements = []
    const callBack = (entries) => {
      for (let i = 0; i < entries.length; i++) {
        if (entries[i].isIntersecting) {
          activeId.value = entries[i].target.id
          break
        }
      }
    }

    const observer = new IntersectionObserver(callBack, {
      root: null,
      threshold: 0.5
    })
    setTimeout(() => {
      elements = document.querySelectorAll('h2, h3')

      for (let i = 0; i < elements.length; i++) {
        observer.observe(elements[i])
      }
    }, 150);

    // cleanup of the observer when elements are stopped being observed since this is SAP
    onBeforeUnmount(() => {
      elements.forEach(item => {
        observer.unobserve(item)
      })
    })
  })
</script>

<style scoped>

</style>