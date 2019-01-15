<template>
  <div id="app" class="clearfloat">
     <leftNav></leftNav>
     <div class="main">
    <router-view/>
    </div>
  </div>
</template>
<script>
import leftNav from '@/components/common/leftNav'
export default {
  name: 'app',
  components: {
    leftNav
  }
}
</script>
<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    height: 100%;
  }
  .main{
  float:left;
  width:95%; 
  background-color: #EFF2F7;
  height:100%;
  overflow: hidden;
}
body,html{
  padding: 0;
  margin: 0;
  height:100%;
}
.clearfloat:after{display:block;clear:both;content:"";visibility:hidden;height:0}
   .clearfloat{zoom:1}
</style>
