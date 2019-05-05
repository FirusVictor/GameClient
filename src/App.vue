<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
body{
  margin: 0;
  font-family: sans-serif;
  background: #efefef;
}
  input,button{
    outline-style: none;
  }
  ul{
    list-style-type: none;
    margin: 0;
    padding: 0;
  }
</style>
