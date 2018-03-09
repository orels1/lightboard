<template lang="pug">
  #app
    .title ⚡ lightboard
    router-view
</template>

<script>
export default {
  name: 'App',
};
</script>

<style scoped>
.title {
  font-size: 2.5rem;
  margin: 0px 20px 20px 20px;
}
</style>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fcfcfc;
  margin-top: 40px;
}

body {
  background: #21252B;
}
</style>
