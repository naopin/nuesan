<template>
  <header>
    <nav>
      <div id="header">
        <div class="navbar">
          <p class="navbar_log">Shares</p>
          <ul>
            <li>
              <div class="modal-window">
                <button @click="SignInopenModal">SIGNIN</button>
                <MyModal @close="SignIncloseModal" v-if="SignInModal">
                  <SignIn></SignIn>
                </MyModal>
              </div>
            </li>
            <li>
              <div class="modal-window">
                <button @click="SignOutopenModal">SIGNUP</button>
                <MyModal @close="SignOutcloseModal" v-if="SignOutModal">
                  <SignUp></SignUp>
                </MyModal>
              </div>
            </li>
          </ul>
        </div>
      </div>
    </nav>
    <!-- <SelctMenu></SelctMenu> -->
  </header>
</template>
<script>
// import SelctMenu from "../components/SelctMenu";
import MyModal from "../components/Mymodal";
import SignIn from "../components/SignIn";
import SignUp from "../components/SignUp";
export default {
  components: {  MyModal, SignIn, SignUp },
  name: "Header",
  data() {
    return {
      SignInModal: false,
      SignOutModal: false
    };
  },
  methods: {
    SignInopenModal() {
      this.SignInModal = true;
    },
    SignIncloseModal() {
      this.SignInModal = false;
    },
    SignOutopenModal() {
      this.SignOutModal = true;
    },
    SignOutcloseModal() {
      this.SignOutModal = false;
    }
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

.modal-window {
  background: #f6f6f6;
}

button {
  color: blue;
  border: none; /* 枠線を消す */
  outline: none; /* クリックしたときに表示される枠線を消す */
  background: transparent;
  text-shadow: 2px 2px 3px #acabab;
  cursor: pointer;
}


button:hover {
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