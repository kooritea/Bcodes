<template>
  <div id="app">
    <div id="bcodes">
      <Bcodes v-bind:code-config="config" />
    </div>
  </div>
</template>

<script>
import Bcodes from './components/bcodes'

export default {
  name: 'App',
  components: {
    Bcodes
  },
  data:function(){
    return {
      config:{
        getCode:"http://localhost:8088",
        checkCode:""
      }
    }
  }
}
</script>

<style>
body{
  margin: 0;
  background-color: #000;
}
#app{
  position: relative;
  width: 100vw;
  height: 100vh;
}
#bcodes{
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
}
</style>
