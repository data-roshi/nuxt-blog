<template>
  <div class="not-prose">
    <section v-if="pending">loading...</section>
    <section v-else-if="error">something went wrong...</section>
    <section v-else>
      <ul class="grid grid-cols-1 gap-4">
        <li v-for="repo in repos" :key="repo.id" class="border border-gray-200 dark:border-gray-800 rounded-sm p-4 hover:bg-gray-200 dark:hover:bg-gray-800 font-mono">
          <a :href="repo.html_url" target="_blank">
            <div class="flex items-center justify-between text-sm">
              <div class="font-semibold">{{ repo.name }}</div>
              <div>{{ repo.stargazers_count }} ★</div>
            </div>
            <p class="text-sm">
              {{ repo.description }}
            </p>
          </a>
        </li>
      </ul>
    </section>
  </div>
</template>
<script setup lang="ts">
type Repo = {
  id: string;
  name: string;
  html_url: string;
  description: string;
  stargazers_count: number;
};
const {error, pending, data} = await useFetch<Repo[], Error>( 'https://api.github.com/users/piotr-jura-udemy/repos', { lazy: true });

const repos = computed(() =>
    (data.value as Repo[])
    .filter((repo: Repo) => !!repo.description)
    .sort((a: Repo, b: Repo) => b.stargazers_count - a.stargazers_count ));
</script>