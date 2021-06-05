<template>
  <div class="author_info">
    

    <div class="author_summary">
      <div class="top_bar">作者</div>
      <router-link
        :to="{
          name: 'user_info',
          params: {
            name: userinfo.loginname
          }
        }"
      >
        <img :src="userinfo.avatar_url" alt="" />
        <span>{{ userinfo.loginname }}</span>
      </router-link>
      <div class="score">积分:{{ userinfo.score }}</div>
    </div>
    <div class="recent_topic">
      <div class="top_bar">作者最近主题</div>
      <ul>
        <li v-for="post in topicLimitFive" :key="post.id" >
          <!--标题-->
          <!-- 此时直接点击路由不跳转 -->
          <!-- 此时点击标题只是参数的变化 路径没有变化 -->
          <!-- 原因 在同一个路由内是检测不到这种变化的 -->
          <span class="post_list_title" >
            <router-link 
            :to="{
              name: 'post_content',
              params: {
                id: post.id,
                name: post.author.loginname
              }
            }"
          >
            {{ post.title }}
            </router-link>
          </span>
        </li>
      </ul>
    </div>
    <div class="recent_reply">
      <div class="top_bar">作者最近回复</div>
      <ul>
        <li v-for="post in replyLimitFive" :key="post.id">
          <span class="post_list_title" >
            <router-link
            :to="{
              name: 'post_content',
              params: {
                id: post.id,
                name: post.author.loginname
              }
            }"
          >
            {{ post.title }}
            </router-link>
          </span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "SideBar",

  data() {
    return {
      userinfo: {},
    };
  },

  methods: {
    getUserData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          // console.log(res);
          this.isLoading = false;
          this.userinfo = res.data.data;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },

  // 计算属性 处理 最多显示5条信息
  computed: { 
    topicLimitFive() { 
      if(this.userinfo.recent_topics){
        return this.userinfo.recent_topics.slice(0, 5)
      }
  },
    replyLimitFive() { 
      if(this.userinfo.recent_replies){
        return this.userinfo.recent_replies.slice(0, 5)
      }
  }
  },
  // 
  beforeMount() {
    this.getUserData();
  },
  watch: {
    $route (to, from) {
      // 对路由变化作出响应...
      this.getUserData();
    }
  },
}
</script>

<style scoped>
.author_info {
  float: right;
  width: 19%;
}
.author_info a {
  text-decoration: none;
  color: #778087;
  font-size: 14px;
}
.author_info li {
  list-style: none;
  font-size: 14px;
  color: #778087;
  margin: 5px;
}
.author_summary .score {
  font-size: 14px;
  color: #778087;
}
.author_summary img {
  width: 48px;
  height: 48px;
}
.author_summary a {
  text-decoration: none;
  color: #778087;
  font-size: 14px;
}

.author_summary,
.recent_topic,
.recent_reply {
  background-color: white;
  margin-bottom: 20px;
  padding: 10px;
}
.top_bar {
  height: 40px;
  line-height: 40px;
  background-color: #f6f6f6;
  font-size: 14px;
  color: #494949;
}
</style>