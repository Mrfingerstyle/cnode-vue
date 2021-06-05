<template>
  <div class="article">
    <!-- 加载等待动画 -->
    <div class="isLoading" v-if="isLoading">
      <img src="../assets/loading.gif" alt="" />
    </div>

    <div v-else>
      <div class="topic_header">
        <div class="topic_title">
          {{ post.title }}
        </div>
        <ul>
          <li>发布于{{ post.create_at | formatDate }}</li>
          <li>作者 {{ post.author.loginname }}</li>
          <li>{{ post.visit_count }}次浏览</li>
          <li>来自 {{ post.tab | tabFormatter }} 板块</li>
        </ul>
      </div>
      <hr />
      <div class="topic_content" v-html="post.content"></div>
      <div class="topic_reply">
        <div class="reply_count">{{ post.replies.length }} 条回复</div>
        <div
          class="reply_list"
          v-for="(reply, index) in post.replies"
          :key="index"
        >
          <div class="reply_person_intro">
            <router-link
              :to="{
                name: 'user_info',
                params: {
                  name: reply.author.loginname
                }
              }"
            >
              <img :src="reply.author.avatar_url" alt="" />
            </router-link>
            <router-link
              :to="{
                name: 'user_info',
                params: {
                  name: reply.author.loginname
                }
              }"
            >
              <span class="name">{{ reply.author.loginname }}</span>
            </router-link>
            <span class="index">{{ index + 1 }}楼</span>
            <span class="create_at_time">{{
              reply.create_at | formatDate
            }}</span>
            <span class="ups" v-if="reply.ups.length > 0"
              ><img src="../assets/up.png" alt="" />&nbsp;{{
                reply.ups.length
              }}</span
            >
            <span v-else></span>
            <span v-if="reply.id === post.id" class="isAuthor">作者</span>
            <span v-else></span>
          </div>
          <div class="reply_content" v-html="reply.content"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Article",
  data() {
    return {
      isLoading: false,
      // 代表当前文章页的所有内容
      post: {}
    };
  },
  methods: {
    getArticleData() {
      this.$http
        .get(`https://cnodejs.org/api/v1/topic/${this.$route.params.id}`)
        .then(res => {
          // console.log(res);
          if (res.data.success) {
            this.isLoading = false;
            this.post = res.data.data;
          }
        })
        .catch(err => {
          console.log(err);
        });
    }
  },
  beforeMount() {
    this.isLoading = true;
    this.getArticleData();
  },
  watch: {
    $route (to, from) {
      // 对路由变化作出响应...
      this.getArticleData();
    }
  },
};
</script>

<style>
/* 引入markdown中的css样式 */
/* 注意此时的scope要去掉 */
@import url("../assets/markdown-github.css");
.article {
  background-color: white;
  
  width: 80%;
  float: left;
}
.topic_title {
  font-size: 22px;
  margin: 8px 0;
}
.topic_header li {
  list-style: none;
  display: inline-block;
  color: #8d8d8d;
  font-size: 12px;
  line-height: 20px;
}
.topic_content {
  margin-bottom: 30px;
  color: #333;
}
.topic_content img {
  width: 70%;
  height: 70%;
  position: relative;
  left: 15%;
}
.reply_count {
  height: 40px;
  line-height: 40px;
  background-color: #e1e1e1;
  font-size: 14px;
  /* color: #494949; */
}
.reply_list img {
  width: 30px;
  height: 30px;
  position: relative;
  top: 5px;
}
.reply_list .ups {
  float: right;
  margin-right: 30px;
  position: relative;
  top: 8px;
}
.reply_list .ups img {
  width: 15px;
  height: 15px;
  top: -1px;
}
.reply_list .name {
  color: #666;
}
.reply_list .index,
.reply_list .create_at_time {
  color: #08e;
}
.reply_person_intro {
  line-height: 30px;
  font-size: 14px;
}
.reply_person_intro .isAuthor {
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
.reply_list .reply_content {
  font-size: 15px;
  margin: 10px 0;
  color: #494949;
}
</style>
