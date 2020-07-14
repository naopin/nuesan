<template>
  <div id="signin">
    <h2>ログイン画面</h2>
    <table>
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
    </table>
    <br />
    <button class="login" @click="login">ログイン</button>
    <button @click="resetPassword()" class="resetPassword">パスワードを忘れた</button>
  </div>
</template>

<script>
import firebase from "firebase";

export default {
  name: "SignUp",
  data() {
    return {
      email: "",
      password: ""
    };
  },
  methods: {
    resetPassword: function() {
      this.email = window.prompt(
        "ご登録されたメールアドレスをご入力下さい",

      );
      firebase.auth().languageCode = "ja"; // 日本語に設定
      firebase
        .auth()
        .sendPasswordResetEmail(this.email)
        .then(() => {
          this.email = "";
          alert("パスワード再発行URLを送信しました。");
        })
        .catch(error => {
          alert(error, "該当するデータが見つかりませんでした。");
        });
    },
    login: function() {
      firebase
        .auth()
        .signInWithEmailAndPassword(this.email, this.password)
        .then(() => {
          // this.$router.push("/");
        })
        .catch(error => {
          alert(error.message);
        });
      this.email = "";
      this.password = "";
    },
  }
};
</script>


<style scoped>
#signin h2 {
  margin-bottom: 1em;
}
#signin {
  font-family: "Avenir", "Helvetica", "Arial", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  /* position: absolute;
  top: 40%;
  left: 50%;
  transform: translateY(-50%) translateX(-50%); */
  padding: 3rem;
}

table {
  margin: 0 auto;
  /* font-size: 1.0em; */
}

::placeholder {
  font-size: 0.8em;
}

button {
  color: rgb(255, 255, 255);
  padding: 8px 15px;
  border-radius: 5px;
  /* font-size: 0.8em; */
  background: #1ab6ff;
  box-shadow: 5px 5px 6px -3px rgba(0, 0, 0, 0.2);
  font-weight: 300;
}

.login{
  margin-bottom:1rem ;  
}

td {
  padding: 0.8rem;
}

input {
  width: 100%;
  border-radius: 6px;
}

.resetPassword {
  margin: 0 auto;
  display: block;
  color: rgb(97, 97, 97);
  font-size: 0.6em;
  border: none; /* 枠線を消す */
  outline: none; /* クリックしたときに表示される枠線を消す */
  background: transparent;
  cursor: pointer;
}
</style>
