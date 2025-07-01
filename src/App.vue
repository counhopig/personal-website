<template>
  <div id="app">
    <my-website />
  </div>
  <div class="bg-mask"></div>
  <div class="bg-image"></div>
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
:root {
  --ctp-mocha-base: #1e1e2e;
  --ctp-mocha-mantle: #181825;
  --ctp-mocha-surface0: #313244;
  --ctp-mocha-surface1: #45475a;
  --ctp-mocha-text: #cdd6f4;
  --ctp-mocha-subtext1: #bac2de;
  --ctp-mocha-subtext0: #a6adc8;
  --ctp-mocha-pink: #f5c2e7;
  --ctp-mocha-mauve: #cba6f7;
  --ctp-mocha-red: #f38ba8;
  --ctp-mocha-green: #a6e3a1;
  --ctp-mocha-yellow: #f9e2af;
  --ctp-mocha-blue: #89b4fa;
  --ctp-mocha-flamingo: #f2cdcd;
  --ctp-mocha-rosewater: #f5e0dc;
  --ctp-mocha-overlay0: #6c7086;
  --ctp-mocha-overlay1: #7f849c;
  --ctp-mocha-overlay2: #9399b2;
}

#app {
  position: relative;
  z-index: 2;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  color: var(--ctp-mocha-text);
  background: linear-gradient(
    135deg,
    var(--ctp-mocha-base) 60%,
    var(--ctp-mocha-surface0) 100%
  );
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.bg-image {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100vw;
  height: 100vh;
  background: url("https://images.unsplash.com/photo-1506744038136-46273834b3fb?auto=format&fit=crop&w=1500&q=80")
    center center/cover no-repeat;
  z-index: 0;
}

.bg-mask {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: 100vw;
  height: 100vh;
  background: rgba(30, 30, 46, 0.55);
  z-index: 1;
}

header {
  padding-top: 60px;
  position: relative;
  top: 0;
  z-index: 3;
}

h1 {
  color: var(--ctp-mocha-pink);
  letter-spacing: 2px;
  text-shadow: 0 2px 8px var(--ctp-mocha-mantle);
  margin-bottom: 0.5em;
}

@media (max-width: 600px) {
  #app {
    padding: 0 8px;
  }
  header {
    padding-top: 30px;
  }
  h1 {
    font-size: 1.2em;
  }
}
</style>
