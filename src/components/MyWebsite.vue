<template>
  <div class="website-list">
    <h2>My Personal Tools Website</h2>
    <ul>
      <li
        v-for="site in websites"
        :key="site.url"
        @click="goToSite(site.url)"
        class="clickable"
      >
        <a :href="site.url" target="_blank">{{ site.name }}</a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "WebsiteList",
  data() {
    return {
      websites: [
        { name: "IT Tools", url: "https://it-tool.counhopig.top" },
        { name: "Wakapi", url: "https://wakapi.counhopig.top" },
        { name: "PDF Tools", url: "https://pdf.counhopig.top" },
      ],
    };
  },

  methods: {
    goToSite(url) {
      window.open(url, "_blank");
    },
  },
};
</script>

<style scoped>
.website-list {
  max-width: 1200px;
  margin: 40px auto;
  padding: 20px;
  background: #1e1e2e;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
  border-radius: 12px;
  border: 2px solid #ffffff;
}

.website-list h2 {
  text-align: center;
  color: #cdd6f4;
  margin-bottom: 20px;
}

.website-list ul {
  list-style: none;
  padding: 0;
}

.website-list li {
  margin: 10px 0;
  background-color: #313244;
  padding: 10px 20px;
  border-radius: 8px;
  transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
}

.website-list li:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.website-list a {
  color: #f28fad;
  text-decoration: none;
  font-weight: bold;
}

.website-list a:hover {
  text-decoration: underline;
  color: #abe9b3;
}
</style>
