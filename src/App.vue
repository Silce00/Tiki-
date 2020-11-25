<template>
  <div id="app">

    <router-view></router-view>
  </div>
</template>

<script>
export default {

 
}
</script>

<style lang="less">
body{
  background-color: #f7f7f7;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.auto-img{
  width: 100%;
  display: block;
}
.clearfix::after{
  content: "";
  display: block;
  clear: both;
}
.fl{
  float: left;
}
.fr{
  float: right;
}

.one-text{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
