<template>
  <div class="home">
    <div v-if="!navbar">
      <Header></Header>
    </div>
    <div v-if="navbar">
      <HeaderSignIn></HeaderSignIn>
    </div>
      <SelctMenu></SelctMenu>
  </div>
</template>


<script>
import Header from "@/components/Header.vue";
import HeaderSignIn from "@/components/HeaderSignIn.vue";
import firebase from "firebase";
import SelctMenu from "../components/SelctMenu"


export default {
  components: { Header,  HeaderSignIn, SelctMenu},
  name: "Home",
  data() {
    return {
      navbar: false,
    };
  },
  created() {
    this.$nextTick(function() {
      firebase.auth().onAuthStateChanged((user)=> {
        if (user) {
          // console.log("ログイン中", user);
          this.navbar = true;
        } else {
          // console.log("ログインアウト中");
          this.navbar = false;
          // console.log(this.navbar);
        }
      });
    });
    // console.log(this.navbar); // =null
  }
};
</script>
