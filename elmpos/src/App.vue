<template>
  <div id="app">
    <LeftNav></LeftNav>
    <div class="pos-main">
        <router-view></router-view>
    </div>
  </div>
</template>

<script>
import LeftNav from '@/components/LeftNav'

export default {
  name: 'app',
  components: {
    LeftNav
  }
}
</script>

<style>
#app {
  font-family: 'Microsoft YaHei', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.pos-main{
  float: left;
  width: 94%;
  height: 100%;
  background-color: #EFF2F7;
}
</style>
