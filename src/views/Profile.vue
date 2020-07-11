<template>
  <div id="profile">
    <Header></Header>
    <div class="profile">
      <div class="profile_modify">
        <ProfileModify></ProfileModify>
      </div>

      <div class="movie_history">
        <h1 class="movie_history_title">投稿履歴</h1>
        <div
          class="childItems"
          v-for="item in allHistoryMovie"
          :key="item.snippet.title"
        >
          <div class="history_cards">
            <img
              class="thumbnails"
              v-bind:src="item.snippet.thumbnails.medium.url"
            />
            <h2>{{ item.snippet.title }}</h2>
          </div>
          <button @click="deleteBtn(item)">削除</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ProfileModify from "../components/ProfileModify";
import Header from "../components/HeaderSignIn";
import "firebase/auth";
import "firebase/firestore";
import { firebaseApp } from "../main";
import * as firebase from "firebase/app";

export default {
  components: { Header, ProfileModify },
  data() {
    return {
      allHistoryMovie: [],
    };
  },
  created() {
    this.$nextTick(function() {
      const self = this;
      firebaseApp
        .firestore()
        .collection("shares")
        .onSnapshot((querySnapshot) => {
          let videos = [];
          querySnapshot.forEach((doc) => {
            videos = [...videos, doc.data()];
          });
          self.allHistoryMovie = videos;
          console.log("query", self.allHistoryMovie);
        });
    });
  },
  methods: {
    deleteBtn(value) {
      firebase
        .firestore()
        .collection("shares")
        .doc(value.snippet.movieId)
        .delete()
        .then(() => {
          console.log("履歴", this.allHistoryMovie);
        })
        .catch((error) => {
          console.error("Error removing document: ", error);
        });
    },
  },
};
</script>

<style scoped>
.profile {
  margin: 3em 3em;
  padding: 2em 1em;
  box-shadow: 2px 2px 2px 0 rgba(0, 0, 0, 0.2);
  border: 1px solid #eee;
  display: flex;
  justify-content: space-between;
}
h2 {
  padding: 1em;
}
.movie_history_title {
  font-size: 4em;
  text-align: center;
}

.profile_modify {
  width: 50%;
}
.movie_history {
  width: 50%;
}

.childItems {
  width: 70%;
  box-shadow: 2px 2px 2px 0 rgba(0, 0, 0, 0.2);
  border: 1px solid #eee;
  font-size: 1.2em;
  margin: 0 auto;
  margin-bottom: 3em;
}
</style>
