<template>
  <div id="app">
    <header>
      <h1>Welcome to My Website Collection</h1>
    </header>
    <my-website></my-website>
  </div>
</template>

<script>
import MyWebsite from './components/MyWebsite.vue'
export default {
  name: 'App',
  components: {
    'my-website': MyWebsite
  }
}
</script>

<style>
#app {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
  text-align: center;
  color: #CDD6F4; 
  background-color: #1E1E2E; 
  min-height: 100vh; 
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

header {
  padding: 20px;
}

h1 {
  color: #F28FAD; 
}
</style>
