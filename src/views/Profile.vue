<template>
  <div id="profile">
    <!-- <Header></Header> -->
    <div class="profile_modifies">
      <div class="profile_title">
        <h1>ユーザー情報の編集</h1>
      </div>
      <div class="profile_modify">
        <div class="current_user_information" v-if="currentUserInformation">
          <div class="user_name">
            <h3>ユーザー名</h3>
            <h4>{{displayUsers.username}}</h4>
          </div>
          <div class="user_email">
            <h3>メールアドレス</h3>
            <h4>{{displayUsers.email}}</h4>
          </div>
          <button @click="profilelModify()">プロフィール変更</button>
        </div>
        <div class="edit_profile" v-if="editProfile">
          <h3>ユーザー名</h3>
          <h4>{{displayUsers.username}}</h4>
          <input type="text" :value="displayUsers.username" @input="userNameModify" />
          <button @click="saveUserName()">変更</button>

          <h3>メールアドレス</h3>
          <h4>{{displayUsers.email}}</h4>
          <input type="text" :value="displayUsers.email" @input="emailModify" />
          <button @click="saveEmail()">変更</button>
          <button @click="cancel()">キャンセル</button>
        </div>
      </div>
    </div>
  </div>
</template>



<script>
import firebase from "firebase";
import "firebase/auth";
import "firebase/firestore";
// import Header from "../components/HeaderSignIn";
export default {
  // components: { Header },
  data() {
    return {
      currentUserReference: [],
      currentUsers: [],
      dbUsers: [],
      currentUserInformation: true,
      editProfile: false,
      updateUserName: "",
      db: [],
      usersRef: [],
      newMail: "",
      displayUsers: []
    };
  },
created() {
  const self = this
    this.$nextTick(function () {
      //データベースに保管されているユーザー
      self.db = firebase.firestore();
      self.usersRef = self.db.collection("users");
      //ログインユーザーを参照
      firebase.auth().onAuthStateChanged(function (user) {
        if (user) {
          self.currentUserReference = user;
          self.dbUsers = self.usersRef.where(
            "uid",
            "==",
            self.currentUserReference.uid
          );
          self.dbUsers.get().then((querySnapshot) => {
            querySnapshot.forEach((doc) => {
              // ok
                  self.displayUsers = doc.data()
            });
          });
        } else {
          console.log("ログインユーザーを参照できていません");
        }
        self.usersRef.onSnapshot((snapshot) => {
        snapshot.docs.forEach(() => {
            if (user) {
              self.dbUsers = self.usersRef.where(
                "uid",
                "==",
                self.currentUserReference.uid
              );
              self.dbUsers.get().then((querySnapshot) => {
                querySnapshot.forEach((doc) => {
                  // ok
                  self.displayUsers = doc.data()
                });
              });
            }
          });
        });
      });
    });
  },
  methods: {
    profilelModify() {
      this.currentUserInformation = false;
      this.editProfile = true;
    },
    cancel() {
      this.editProfile = false;
      this.currentUserInformation = true;
    },
    //変更されたユーザー名を取得
    userNameModify(e) {
      this.userName = e.target.value;
    },
    //Firestoreのユーザー名を変更
    saveUserName() {
      if (this.userName) {
        firebase.firestore().collection("users")
          .doc(this.displayUsers.uid)
          .update({ username: this.userName })
          .then(() => {
            firebase
              .auth()
              .currentUser.updateProfile({
                displayName: this.userName
              })
              .then(function() {
                console.log("現在のユーザー情報", firebase.auth().currentUser);
              })
              .catch(function() {
                // An error happened.
              });
          })
          .catch(error => {
            console.error("Error edit user: ", error);
          });

        // this.$router.go({path: this.$router.push("/profile"), force: true}
        // this.currentUserReference = firebase.auth().currentUser;
        this.cancel();
      } else {
        alert("変更が確認されません");
      }
    },

    //変更されたメールアドレスを取得
    emailModify(e) {
      this.newMail = e.target.value;
    },
    //firebaseのメールを更新
    saveEmail() {
      const user = firebase.auth().currentUser;
      user
        .updateEmail(this.newMail)
        .then(function() {
          // Update successful.
          console.log("seikou");
        })
        .catch(function() {
          // An error happened.
        });
    }
  }
};
</script>

<style scoped>
.profile_modifies {
  margin: 3em 3em;
  font-size: 2em;
  padding: 2em 1em;
  box-shadow: 2px 2px 2px 0 rgba(0, 0, 0, 0.2);
  border: 1px solid #eee;
}
</style>