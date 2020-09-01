<template>
  <div id="app">
    <h3>掲示板</h3>
    <label for="name">名前:</label>
    <input type="name" id="name" v-model="name" />
    <br />
    <label for="coment">コメント:</label>
    <textarea type="name" id="coment" rows="10" v-model="comment" />
    <br />

    <button @click="createComent">送信</button>
    <h2>掲示板</h2>

    <div v-for="post in posts" :key="post.name">
      <div>名前:{{ post.fields.name.stringValue }}</div>
      <div>コメント:{{ post.fields.comment.stringValue }}</div>
      <br />
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data() {
    return {
      name: "",
      comment: "",
      posts: [],
    };
  },
  //受け取り
  created() {
    axios
      .get(
        "https://firestore.googleapis.com/v1/projects/プロジェク名/databases/(default)/documents/comments"
      )
      .then((response) => {
        this.posts = response.data.documents;
      });
  },
  methods: {
    //送信
    createComent() {
      //第一比数
      axios
        .post(
          "https://firestore.googleapis.com/v1/projects/プロジェクト名/databases/(default)/documents/comments",
          {
            //第２比数
            fields: {
              name: {
                stringValue: this.name,
              },
              comment: {
                stringValue: this.comment,
              },
            },
          }
        )
        //第３比数
        .then((response) => {
          console.log(response);
        })
        .catch((error) => {
          console.log(error);
        });
      this.name = "";
      this.comment = "";
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
