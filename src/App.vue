<template>
  <div id="app">
    <Header></Header>
    <div class="main">
        <router-view name="main" class="left_part"></router-view>
        <router-view name="sidebar" class="right_part"></router-view>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header";
import PostList from "./components/PostList";
export default {
  name: "App",
  // 引入组件
  components: {
    Header,
    PostList
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  background-color: #e1e1e1;
}
.main {
  width: 90%;
  margin: 0 auto;
  background-color: #e1e1e1;
}
/* .left_part {
  float: left;
  width: 80%;
}
.right_part {
  float: right;
  width: 18%;
} */
</style>
