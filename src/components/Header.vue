<template>
  <div class="header">
    <router-link
      :to="{
        name: 'root'
      }"
    >
      <img src="../assets/cnodejs_light.svg" alt="" />
    </router-link>
    <ul>
      <li><a href="#">首页</a></li>
      <li><a href="#">新手入门</a></li>
      <li><a href="#">API</a></li>
      <li><a href="#">关于</a></li>
      <li><a href="#">注册</a></li>
      <li><a href="#">登陆</a></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Header"
};
// scope 表示组件样式的作用只对当前组件中生效
</script>

<style scoped>
.header {
  height: 50px;
  background-color: #444444;
  padding: 0 80px;
}
.header img {
  width: 100px;
  height: 50px;
}
.header ul {
  float: right;
}
.header ul li {
  list-style: none;
  display: inline-block;
  margin: 0 10px;
  line-height: 50px;
}
.header ul li a {
  color: #e3e3e3;
  text-decoration: none;
  font-size: 13px;
}
.header ul li a:hover {
  color: white;
}
</style>
