<template>
  <div class="UserInfo">
    <!-- 加载等待占位图片 -->
    <div class="isLoading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="" />
    </div>
    <div v-else class="user_infor_content">
      <section class="user_infor">
        <div>
          <img :src="post.avatar_url" alt="" />
          <span>{{ post.loginname }}</span>
        </div>
        <div class="score">
          <span>积分{{ post.score }}</span>
        </div>
        <div>
          <span>注册时间{{ post.create_at | formatDate }}</span>
        </div>
      </section>
      <div class="recent_topics">
        <div class="create_topics">最近创建的主题</div>
        <ul>
          <li v-for="post in post.recent_topics" :key="post.id">
            <!--头像-->
            <img :src="post.author.avatar_url" alt="" />&nbsp;&nbsp;
            <!--回复/浏览-->

            <!--帖子的分类-->
            <router-link
              :to="{
                name: 'post_content',
                params: {
                  id: post.id
                }
              }"
            >
              <span class="post_list_title">
                {{ post.title }}
              </span>
            </router-link>
            <!--最終回复时间-->
            <span class="last_reply">
              {{ post.last_reply_at | formatDate }} </span
            >&nbsp;&nbsp;
          </li>
        </ul>
      </div>
      <div class="recent_replies">
        <div class="reply_topics">最近回复的主题</div>
        <ul>
          <li v-for="post in post.recent_replies" :key="post.id">
            <!--头像-->
            <img :src="post.author.avatar_url" alt="" />&nbsp;&nbsp;
            <!--回复/浏览-->

            <!--帖子的分类-->

            <!--标题-->
            <router-link
              :to="{
                name: 'post_content',
                params: {
                  id: post.id
                }
              }"
            >
              <span class="post_list_title">
                {{ post.title }}
              </span>
            </router-link>
            <!--最終回复时间-->
            <span class="last_reply">
              {{ post.last_reply_at | formatDate }} </span
            >&nbsp;&nbsp;
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "UserInfo",
  data() {
    return {
      isLoading: false,
      post: {},
      number: []
    };
  },
  methods: {
    getUserData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/user/${this.$route.params.name}`)
        .then(res => {
          // console.log(res);
          this.isLoading = false;
          this.post = res.data.data;
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  beforeMount() {
    (this.isLoading = true), this.getUserData();
  }
};
</script>

<style scoped>
.user_infor {
  height: 120px;
  background-color: white;
  margin: 0 auto;
  padding-left: 100px;
  margin-bottom: 10px;
}
.recent_topics {
  margin-bottom: 10px;
}
.user_infor_content li {
  list-style: none;
  height: 50px;
  background-color: white;
}
.user_infor_content img {
  position: relative;
  width: 30px;
  height: 30px;
  top: 10px;
  padding-left: 3px;
}
.user_infor_content a {
  text-decoration: none;
  color: #494949;
}

.user_infor_content .reply_topics {
  height: 40px;
  line-height: 40px;
  background-color: #f6f6f6;
  font-size: 14px;
  color: #494949;
}
.user_infor_content .create_topics {
  height: 40px;
  line-height: 40px;
  background-color: #f6f6f6;
  font-size: 14px;
  color: #494949;
}
.user_infor .score {
  margin: 18px 0 12px 0;
}
.user_infor_content {
  font-size: 14px;
}
.user_infor_content .last_reply {
  float: right;
  margin-right: 20px;
  color: #909090;
  position: relative;
  top: 15px;
}
.user_infor_content .create_topics,
.user_infor_content .reply_topics {
  padding-left: 20px;
}
</style>
