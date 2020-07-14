<template>
  <header>
    <nav>
      <div id="header">
        <div class="navbar">
          <p class="navbar_log">Shares</p>
          <ul>
            <li>
              <router-link to="/">HOME</router-link>
            </li>
            <li>
              <router-link to="/share">SHARE</router-link>
            </li>
            <li>
              <router-link to="/profile">PROFILE</router-link>
            </li>
            <li>
              <button class="signoutbutton" @click="logout">SIGNOUT</button>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <!-- <SelctMenu></SelctMenu> -->
  </header>
</template>


<script>
import firebase from "firebase";
// import SelctMenu from "../components/SelctMenu"
export default {
  // components: {SelctMenu},
  name: "Header",
  methods: {
    logout: function() {
      firebase
        .auth()
        .signOut()
        .then(() => {
          //   this.$router.push("/");
        })
        .catch(error => {
          alert(error.message);
        });
    }
  },
  created() {
    this.$nextTick(function() {
      firebase.auth().onAuthStateChanged(user => {
        if (user) {
          console.log("ログイン中", user);
        } else {
          console.log("ログインアウト中");
        }
      });
    });
    // console.log(this.navbar); // =null
  }
};
</script>

<style scoped>
#header {
  background: #f6f6f6;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.2);
  font-family: "Gabriola", "Skia-Regular_Condensed", "Noteworthy", sans-serif;
}
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
ul {
  display: flex;
  list-style: none;
}
li {
  font-size: 2.5em;
  text-decoration: none;
  padding: 1rem;
}
.signoutbutton {
  color: blue;
  border: none; /* 枠線を消す */
  outline: none; /* クリックしたときに表示される枠線を消す */
  background: transparent;
  text-shadow: 2px 2px 3px #acabab;
}

a {
  color: blue;
  text-shadow: 2px 2px 3px #acabab;
}

a:hover,
.signoutbutton:hover {
  color: rgb(160, 160, 160);
}

.navbar_log {
  font-size: 4.5em;
  font-weight: 300;
  margin: 0;
  padding: 1rem;
  text-shadow: 2px 2px 3px #acabab;
}
</style>