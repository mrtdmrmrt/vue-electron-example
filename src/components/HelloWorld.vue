<template>
  <div class="hello">
    <ul class="list-group list-group-flush">
      <li
        class="list-group-item flex-container"
        v-for="post in posts"
        :key="post.data.id"
        @click="openImage(post.data.thumbnail)"
      >
        <img :src="post.data.thumbnail" alt="thumb" class="thumbnail" />
        <div>{{ post.data.title }}</div>
      </li>
    </ul>
  </div>
</template>

<script>
const axios = require("axios");

import { ipcRenderer } from "electron";
export default {
  name: "HelloWorld",
  data() {
    return {
      posts: [],
    };
  },
  props: {
    msg: String,
  },
  created() {
    axios
      .get("https://reddit.com/r/aww.json")
      .then((response) => {
        this.posts = response?.data?.data?.children;
      })
      .catch((err) => {
        console.log("err", err);
      });
  },
  methods: {
    openImage(image) {
      ipcRenderer.send("toggle-image", image);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.list-group {
  display: flex;
  flex-direction: column;
  padding-left: 0;
  margin-bottom: 0;
  border-radius: 0.25rem;
}
.list-group-flush .list-group-item {
  border-right: 0;
  border-left: 0;
  border-radius: 0;
}
.list-group-flush:first-child .list-group-item:first-child {
  border-top: 0;
}
.list-group-item {
  position: relative;
  display: block;
  padding: 0.5rem 1rem;
  color: #212529;
  text-decoration: none;
  background-color: #fff;
  border: 1px solid rgba(0, 0, 0, 0.125);
  cursor: pointer;
}
.list-group-item:hover {
  background-color: #eee;
}
.flex-container {
  display: flex;
  align-items: center;
}
.thumbnail {
  width: 60px;
  height: 60px;
  border-radius: 30px;
  margin-right: 16px;
}
</style>
