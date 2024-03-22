<template>
  <div id="app">
    <header>
      <h1>Hi, this is Counhopig</h1>
      <h1>Welcome to My Website Collection</h1>
    </header>
    <my-website> </my-website>
  </div>
</template>

<script>
import MyWebsite from "./components/MyWebsite.vue";
export default {
  name: "App",
  components: {
    "my-website": MyWebsite,
  },
};
</script>

<style>
#app {
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
  color: #cdd6f4;
  background-color: #1e1e2e;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  position: relative;
}

header {
  padding: 20px;
  position: relative;
  top: 0;
}

my-website {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 20px;
  padding: 20px;
  width: 1200px;
  margin: 0 auto;
}

h1 {
  color: #f28fad;
}
</style>
