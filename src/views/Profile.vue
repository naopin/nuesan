<template>
  <div id="profile">
    <Header></Header>
    <div class="profile">
      <div class="profile_modify">
        <ProfileModify></ProfileModify>
      </div>

      <div class="movie_history">
        <h1 class="movie_history_title">投稿履歴</h1>
        <div class="childItems" v-for="item in allHistoryMovie" :key="item.snippet.title">
          <div class="history_cards">
            <img class="thumbnails" v-bind:src="item.snippet.thumbnails.medium.url" />
            <h2>{{item.snippet.title}}</h2>
          </div>
          <button class="delete_btn" @click="deleteBtn(item)">削除</button>
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
      allHistoryMovie: []
    };
  },

  created() {
    this.$nextTick(function() {
      const self = this;
      firebase.auth().onAuthStateChanged(user => {
        if (user) {
          firebaseApp
            .firestore()
            .collection("shares")
            //firestoreのすべてdoc取得
            .onSnapshot(querySnapshot => {
              let videos = [];
              querySnapshot.forEach(doc => {
                videos.push(doc.data());
              });
              let filterUid = [];
              //ログインユーザーの投稿のみ(doc)取得
              filterUid = videos.filter(item => {
                return item.userId === user.uid;
              });
              self.allHistoryMovie = filterUid;
            });
        } else {
          self.$router.push("/");
        }
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
        .catch(error => {
          console.error("Error removing document: ", error);
        });
    }
  }
};
</script>

<style scoped>
.profile {
  margin: 2em 2em;
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

.delete_btn {
  color: #eee;
  background: rgb(255, 13, 13);
  padding: 0.5em 1em;
  font-size: 1.3em;
  font-weight: bold;
  border-radius: 6px;
  border-bottom: solid 4px #6b6b6b;
  margin: 0.5em;
  border-top: none;
  border-left: none;
  border-right: none;
  cursor: pointer;
  outline: none;
  appearance: none;
}
.delete_btn:hover {
  background: rgb(255, 103, 103);
}

.delete_btn:active {
  -webkit-transform: translateY(3px);
  transform: translateY(3px); /*下に動く*/
  border-bottom: none; /*線を消す*/
}
</style>