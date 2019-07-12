<style scoped>
.container {
  text-align: center;
  display: flex;
  flex-direction: column;
}
.posts-container {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
  overflow-x: auto;
}

.post {
  margin: 5px 5px;
  cursor: pointer;
}

.title {
  font-size: 1.5rem;
  font-weight: bold;
}

.subtitle {
  margin: 10px;
}
</style>
<template>
  <div class="container">
    <span class="title">{{title}}</span>
    <span class="subtitle">{{subtitle}}</span>
    <div class="posts-container">
      <div v-for="(post, index) in feed" :key="index" class="post">
        <a :href="post.link" :aria-label="post.caption.text" target="_blank" rel="noopener"><img :src="post.images.thumbnail.url" alt=""></a>
      </div>
    </div>
  </div>
</template>
<script>
const axios = require('axios')

export default {
  name: 'InstagramFeed',
  props: {
    token: String,
    title: String,
    subtitle: String
  },
  data () {
    return {
      feed: []
    }
  },
  mounted () {
    this.getUserFeed()
  },
  methods: {
    getUserFeed () {
      axios.get('https://api.instagram.com/v1/users/self/media/recent', {
        params: {
          access_token: this.token
        }
      }).then((response) => {
        this.feed = response.data.data
      }).catch((error) => {
        console.log(error)
      })
    }
  }
}
</script>
