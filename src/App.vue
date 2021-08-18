<template>
  <div class="background flex h-screen antialiased">
    <Inspector />
    <div class="fixed bottom-2 right-2 text-xs">
      <a href="https://noahlitvin.com">noah litvin</a>
    </div>
  </div>
</template>

<script>
import Inspector from "./components/Inspector.vue";

export default {
  name: "App",
  components: {
    Inspector,
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Rubik+Mono+One&family=Rubik:wght@300&display=swap");

html,
body {
  color: #ffffff;
  font-family: "Rubik", sans-serif;
}

h1,
h2,
h3,
h4 {
  font-family: "Rubik Mono One", sans-serif;
}

.background {
  background: linear-gradient(45deg, #00010e 25%, #02085f 100%);
}
a {
  border-bottom: 1px #ffffff dotted;
  border-radius: 1px;
}

input[type="submit"] {
  background: #fdac08;
  color: #00010e;
  height: 38px;
  transform: translateY(1px);
}

input[type="text"] {
  background: transparent;
  border: 1px solid #fdac08;
  width: 100%;
  max-width: 370px;
}

.result-box {
  max-width: 608px;
  background: rgba(0, 0, 0, 0.33);
}
</style>