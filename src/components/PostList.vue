<template>
  <div class="PostList">
    <!-- 内容加载时 等待动画 -->
    <div class="isLoading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="" />
    </div>
    <!-- 主题帖子列表 -->
    <div class="posts" v-else>
      <ul>
        <li>
          <div class="first_line">
            <span><a href="#">全部</a></span>
            <span><a href="#">精华</a></span>
            <span><a href="#">分享</a></span>
            <span><a href="#">回答</a></span>
            <span><a href="#">招聘</a></span>
          </div>
        </li>
        <li v-for="post in posts" :key="post.id">
          <!--头像-->
          <img :src="post.author.avatar_url" alt="" />&nbsp;&nbsp;
          <!--回复/浏览-->
          <span class="allcount">
            <span class="reply_count">{{ post.reply_count }}</span>
            /{{ post.visit_count }} </span
          >&nbsp;&nbsp;
          <!--帖子的分类-->
          <span
            :class="[
              {
                put_good: post.good == true,
                put_top: post.top == true,
                'topiclist-tab': post.good != true && post.top != true
              }
            ]"
          >
            <span>
              {{ post | tabFormatter }}
            </span> </span
          >&nbsp;&nbsp;
          <!--标题-->
          <router-link
            :to="{
              name: 'post_content',
              params: {
                id: post.id,
                name: post.author.loginname
              }
            }"
          >
            <span class="post_list_title">
              {{ post.title }}
            </span>
          </router-link>
          <!--最終回复时间-->
          <span class="last_reply"> {{ post.last_reply_at | formatDate }} </span
          >&nbsp;&nbsp;
        </li>

        <!-- 分页栏 -->
        <li>
          <pagination @handlelist='renderlist'></pagination>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import Pagination from './Pagination'
export default {
  components: { Pagination },
  name: "PostList",
  data() {
    return {
      isLoading: false,
      // 页面内容的列表数组
      posts: [],
      postpage: 1
    };
  },
  // API接口：https://cnodejs.org/api/v1/topics 获取帖子列表
  // API接口：https://cnodejs.org/api/v1/topics?page=1&limit=5
  // 接口 键名解析
  // 帖子发起者头像 author.avatar_url
  // 回复量 浏览量 reply_count visit_count
  // 帖子标题 title
  // 有些字段不能直接获取数据 数据存在分类处理的情况
  // 需要使用vue 过滤器
  // 最后回复时间 last_reply_at
  // 帖子分类 good top tab
  methods: {
    getPostListData() {
      console.log(this.$http);
      this.$http
        .get("https://cnodejs.org/api/v1/topics", {
          params: {
            page: this.postpage,
            limit: 20
          }
        })
        .then(res => {
          // 加载成功去除动画
          this.isLoading = false;
          console.log(res);
          this.posts = res.data.data;
        })
        .catch(err => {
          console.log(err);
        });
    },
    renderlist(value){
      // console.log(value);
      this.postpage = value
      this.getPostListData()
    }
  },
  // 注意 生命周期不能写在函数内
  beforeMount() {
    // 加载成功之前显示加载动画
    this.isLoading = true;
    // 在页面加载之前获取数据
    this.getPostListData();
  }
};
</script>

<style scoped>
.isLoading {
  margin: 20% 50%;
}
.posts {
  font-size: 14px;
}
.posts .first_line {
  height: 40px;
  line-height: 40px;
  background-color: #f6f6f6;
}
.posts .first_line a {
  text-decoration: none;
  color: #80bd34;
  font-size: 13px;
  margin: 0 10px;
}
.posts .first_line a:hover {
  color: #4a86a5;
}
.posts ul {
  list-style: none;
}
.posts a {
  text-decoration: none;
}
.posts ul li {
  height: 50px;
  line-height: 40px;
  background-color: white;
  color: #333;
}

.posts ul li:hover {
  background-color: #f6f6f6;
}

.posts img {
  position: relative;
  width: 30px;
  height: 30px;
  top: 10px;
  margin-left: 10px;
}
.last_reply {
  float: right;
  margin-right: 10px;
  color: grey;
  font-size: 13px;
}
.post_list_title {
  color: #333;
  font-size: 14px;
}
.put_top {
  display: inline-block;
  width: 30px;
  height: 20px;
  text-align: center;
  line-height: 20px;
  border-radius: 3px;
  background-color: #80bd34;
  color: white;
  font-size: 13px;
}
.topiclist-tab {
  display: inline-block;
  width: 30px;
  height: 20px;
  text-align: center;
  line-height: 20px;
  border-radius: 3px;
  background-color: grey;
  color: white;
  font-size: 13px;
}
.allcount {
  background-color: white;
}
</style>
