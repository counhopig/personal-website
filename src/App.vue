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
  align-items: flex-start;
}

header {
  padding: 20px;
  position: relative;
  top: 0;
  left: 20%;
  text-align: left;
}

my-website {
  padding: 20px;
  position: relative;
  left: 20%;
  text-align: left;
}

h1 {
  color: #f28fad;
}
</style>
