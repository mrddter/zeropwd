<template>
  <div>
    <div id="nav">
      <router-link to="/">Password</router-link>
      |
      <router-link to="/about">About</router-link>
    </div>
    <router-view />
  </div>
</template>

<style lang="scss">
body,
html {
  color: #eee;
  background-color: #212325;
}

* {
  color: #eee;
}

#app {
  text-align: center;
  color: #2c3e50;
}

h1 {
  color: #c52e4d;
  margin: 40px 0 0;
  font-size: 1.5rem;
}
h3 {
  color: #9962ac;
  margin: 40px 0 0;
  font-size: 1.2rem;
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
  color: #e34e14;
}

#nav {
  padding: 30px;

  a {
    margin-left: 5px;
    margin-right: 5px;
    text-decoration: none;
    font-size: 1.1rem;
    font-weight: normal;
    color: #eee;

    &.router-link-exact-active {
      color: #e34e14;
      font-weight: normal;
    }
  }
}
</style>
