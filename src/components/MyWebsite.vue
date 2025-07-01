<script>
const message = "Welcome to My Website Collection";
export default {
  name: "MyWebsite",
  data() {
    return {
      message,
    };
  },
};
</script>

<template>
  <div
    class="my-website flex flex-col items-center justify-center text-center p-4"
  >
    <h1 class="text-2xl font-bold mb-4">{{ message }}</h1>
    <p class="text-lg mb-8">
      This is a collection of my favorite websites, built with Vue.js and
      Tailwind CSS.
    </p>
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
      <a
        v-for="(site, index) in sites"
        :key="index"
        :href="site.url"
        target="_blank"
        rel="noopener noreferrer"
        class="bg-white dark:bg-gray-800 rounded-lg shadow-md p-6 hover:shadow-lg transition-shadow duration-300"
      >
        <h2 class="text-xl font-semibold mb-2">{{ site.name }}</h2>
        <p class="text-gray-600 dark:text-gray-400">{{ site.description }}</p>
      </a>
    </div>
  </div>
</template>
