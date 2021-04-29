<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <!-- https://unicode.org/emoji/charts/full-emoji-list.html -->
    <Form />
  </div>
</template>

<script>
// @ is an alias to /src
import Form from '@/components/Form.vue'

export default {
  name: 'Home',
  components: {
    Form,
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only --
<style scoped lang="scss">
*,
html {
  padding: 0;
  margin: 0;
  font-size: 100%;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: magenta;
}
</style>
-->