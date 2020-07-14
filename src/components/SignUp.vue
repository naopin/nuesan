<template>
  <div id="signup">
    <h2>アカウント登録</h2>
    <table>
      <tr>
        <td>
          <label class="label" for="username">ユーザー名</label>
        </td>
        <td>
          <input id="username" type="text" v-model="username" placeholder="ユーザー名をご入力下さい" />
        </td>
      </tr>
      <tr>
        <td>
          <label for="email">メールアドレス</label>
        </td>
        <td>
          <input id="email" type="email" v-model="email" placeholder="メールアドレスをご入力下さい" />
        </td>
      </tr>

      <tr>
        <td>
          <label for="password">パスワード</label>
        </td>
        <td>
          <input id="password" type="password" v-model="password" placeholder="パスワードを入力して下さい" />
        </td>
      </tr>
      <tr>
        <td>
          <label for="passwordConfirm">パスワード確認</label>
        </td>
        <td>
          <input
            id="passwordConfirm"
            type="password"
            v-model="passwordConfirm"
            placeholder="もう一度ご入力下さい"
          />
        </td>
      </tr>
      <tr>
        <td></td>
        <td class="alert">{{alert}}</td>
      </tr>
    </table>
    <br />
    <button @click="register">新規登録</button>
    <br />
  </div>
</template>

<script scoped>
import * as firebase from "firebase/app";
import "firebase/auth";
import "firebase/firestore";
import { firebaseApp } from "../main";

export default {
  name: "SignUp",
  data() {
    return {
      username: "",
      email: "",
      password: "",
      passwordConfirm: "",
      alert: null
    };
  },
  methods: {
    register: function() {
      const validation = (password, passwordConfirm) => {
        if (password === passwordConfirm) {
          return true;
        } else {
          return false;
        }
      };
      if (validation(this.password, this.passwordConfirm)) {
        this.alert = "";
        firebaseApp
          .auth()
          .createUserWithEmailAndPassword(this.email, this.password)
          .then(() => {
            firebaseApp
              .auth()
              .currentUser.updateProfile({ displayName: this.username });

            firebase
              .auth()
              .signInWithEmailAndPassword(this.email, this.password)
              .then(() => {
                const userInfo = firebase.auth().currentUser;
                let db = firebase.firestore();
                // console.log(userInfo);
                db.collection("users")
                  .doc(userInfo.uid)
                  .set({
                    uid: userInfo.uid,
                    username: userInfo.displayName,
                    email: userInfo.email
                  });
                // this.$router.push("/homesignin");
              })
              .catch(error => {
                alert(error.message);
              });

            this.username = "";
            this.email = "";
            this.password = "";
          })
          .catch(error => {
            alert(error.message);
          });
      } else {
        this.alert = "パスワードが一致していません";
      }
    }
  }
};
</script>


<style scoped>
#signup h2 {
  margin-bottom: 1em;
}

#signup {
  font-family: "Avenir", "Helvetica", "Arial", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  padding: 3rem;
}

table {
  margin: 0 auto;
}

::placeholder {
  font-size: 0.8em;
}

button {
  color: rgb(255, 255, 255);
  padding: 8px 15px;
  border-radius: 5px;
  background: #ff801a;
  box-shadow: 5px 5px 6px -3px rgba(0, 0, 0, 0.2);
  font-weight: 300;
}

td {
  padding: 0.8rem;
}

input {
  width: 100%;
  border-radius: 6px;
}
.alert {
  color: red;
  font-size: 0.8em;
}
</style>
