<template>
  <slot :posts="posts">
    <section class="not-prose font-mono">
      <div class="column text-gray-400 text-sm">
        <div>date</div>
        <div>title</div>
      </div>
      <ul>
        <li v-for="post in posts" :key="post._path">
          <NuxtLink :to="post._path"
                    class="column group hover:bg-gray-100 dark:hover:bg-gray-800">
            <div
                :class="{ 'text-white group-hover:text-gray-100 dark:group-hover:text-gray-800 dark:text-gray-900': !post.displayYear, 'text-gray-400 dark:text-gray-500': post.displayYear }">
              {{ post.year }}</div>
            <div>{{ post.title }}</div>
          </nuxtlink>
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
        .where({ _path: { $ne: '/blog' } })
        .only(['_path', 'title', 'publishedAt'])
        .sort({ publishedAt: -1 })

      if (props.limit) {
        query.limit(props.limit);
      }
      return query.find();
    }
)

const posts = computed(() => {
  if (!data.value) {
    return [];
  }

  let lastYear = null;

  for (const post of data.value) {
    console.log(post)
    const year = new Date(post.publishedAt).getFullYear();
    console.log(post.title, year, post.publishedAt)
    post.displayYear = year !== lastYear;
    post.year = year;
    lastYear = year;
  }
  return data.value;
})
</script>

<style scoped>
.column {
  @apply flex items-center flex space-x-8 py-2 border-b border-gray-200 dark:border-gray-700
}
</style>