
<template>
  <div id="app">
    <!-- <img src="./assets/logo.png">
    <router-view/> -->
    <div class="search">
      <search></search>
    </div>
    <!-- <div class="statistic">
      This is statistic
    </div> -->
    <div class="explicit_message">
       详细信息
    </div>
    <div class="detail_list">
      <detail_list></detail_list>
    </div>
  </div>
</template>

<script>
import search from './components/search';
import detail_list from './components/detail_list';
export default {
  name: 'App',
  components: { search,detail_list}
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.explicit_message{
  text-align: left;
  width: 100%;
  height: 2rem;
  border: 1px solid #000;
}

.detail_list{
  float: left;
}
</style>
