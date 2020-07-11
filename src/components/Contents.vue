<template>
  <div id="contents">
    <Search></Search>
    <div id="content">
      <!-- Javascript -->
      <transition name="fade">
        <div v-if="value == 'JavascriptValue'">
          <!-- モーダル -->
          <MyModal @close="javascriptCloseModal()" v-if="jsModal">
            <div class="frames">
              <iframe width="100%" height="100%" :src="jsIframe"></iframe>
            </div>
          </MyModal>
          <div class="parentItems">
            <div class="childItems" v-for="item in jsMapitems" :key="item.url">
              <div @click="javascriptOpenModal(), clickJsItem(item)">
                <img v-bind:src="item.thumbnail" />
                <h2>{{item.title}}</h2>
              </div>
            </div>
          </div>
        </div>
      </transition>
      <!-- vue.js -->
      <transition name="fade">
        <div v-if="value == 'VueValue'">
          <!-- モーダル -->
          <MyModal @close="vueCloseModal()" v-if="vueModal">
            <div class="frames">
              <iframe width="100%" height="100%" :src="vueIframe"></iframe>
            </div>
          </MyModal>
          <div class="parentItems">
            <div class="childItems" v-for="item in vueMapitems" :key="item.url">
              <div @click="vueOpenModal(), clickVueItem(item)">
                <img v-bind:src="item.thumbnail" />
                <h2>{{item.title}}</h2>
              </div>
            </div>
          </div>
        </div>
      </transition>
      <!-- React -->
      <transition name="fade">
        <div v-if="value == 'ReacValue'">
            <MyModal @close="reactCloseModal()" v-if="reactModal">
            <div class="frames">
              <iframe width="100%" height="100%" :src="reactIframe"></iframe>
            </div>
          </MyModal>
          <div class="parentItems">
            <div class="childItems" v-for="item in reactMapitems" :key="item.url">
              <div @click="reactOpenModal(), clickReactItem(item)">
                <img v-bind:src="item.thumbnail" />
                <h2>{{item.title}}</h2>
              </div>
            </div>
          </div>
        </div>
      </transition>
      <!-- Angular -->
      <transition name="fade">
        <div v-if="value == 'AngularValue'">
       <MyModal @close="angularCloseModal()" v-if="angularModal">
            <div class="frames">
              <iframe width="100%" height="100%" :src="angularIframe"></iframe>
            </div>
          </MyModal>
          <div class="parentItems">
            <div class="childItems" v-for="item in angularMapitems" :key="item.url">
              <div @click="angularOpenModal(), clickAngularItem(item)">
                <img v-bind:src="item.thumbnail" />
                <h2>{{item.title}}</h2>
              </div>
            </div>
          </div>
        </div>
      </transition>
      <!-- Node.js -->
      <transition name="fade">
        <div v-if="value == 'NodeValue'">
          <MyModal @close="nodeCloseModal()" v-if="nodeModal">
            <div class="frames">
              <iframe width="100%" height="100%" :src="nodeIframe"></iframe>
            </div>
          </MyModal>
          <div class="parentItems">
            <div class="childItems" v-for="item in nodeMapitems" :key="item.url">
              <div @click="nodeOpenModal(), clickNodeItem(item)">
                <img v-bind:src="item.thumbnail" />
                <h2>{{item.title}}</h2>
              </div>
            </div>
          </div>
        </div>
      </transition>
      <!-- other -->
      <transition name="fade">
        <div v-if="value == 'OtherValue'">
          <MyModal @close="otherCloseModal()" v-if="otherModal">
            <div class="frames">
              <iframe width="100%" height="100%" :src="otherIframe"></iframe>
            </div>
          </MyModal>
          <div class="parentItems">
            <div class="childItems" v-for="item in otherMapitems" :key="item.url">
              <div @click="otherOpenModal(), clickOtherItem(item)">
                <img v-bind:src="item.thumbnail" />
                <h2>{{item.title}}</h2>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>
  </div>
</template>
<script>
// import firebase from "firebase";
import "firebase/auth";
import "firebase/firestore";
import { firebaseApp } from "../main";
import MyModal from "../components/Mymodal";
import Search from "../components/Search";
export default {
  components: { MyModal, Search },
  props: ["value"],
  data() {
    return {
      videoItems: [],
      jsMapitems: "",
      vueMapitems:"",
      reactMapitems:"",
      angularMapitems:"",
      nodeMapitems:"",
      otherMapitems:"",
     //category別Modal
      jsModal: false,
      vueModal: false,
      reactModal: false,
      angularModal: false,
      nodeModal: false,
      otherModal: false,
      //category別の動画
      javascriptItems:[],
      vueItems:[],
      reactItems:[],
      angularItems:[],
      nodeItems:[],
      otherItems:[],
      //category別Iframe
      jsIframe: "",
      vueIframe: "",
      reactIframe: "",
      angularIframe: "",
      nodeIframe: "",
      otherIframe: "",
    };
  },
  created() {
    this.$nextTick(function() {
      const self = this;
      firebaseApp
        .firestore()
        .collection("shares")
        .get()
        .then(querySnapshot => {
          querySnapshot.forEach(doc => {
            self.videoItems.push(doc.data());
          });
          //CategoryがJavascriptの動画を取得
          self.javascriptItems = self.videoItems.filter(item => {
            return item.category === "Javascript";
          });
          //CategoryがVue.jsの動画を取得
          self.vueItems = self.videoItems.filter(item => {
            return item.category === "Vue.js";
          });
              //CategoryがReactの動画を取得
          self.reactItems = self.videoItems.filter(item => {
            return item.category === "React";
          });
          //CategoryがAngularの動画を取得
          self.angularItems = self.videoItems.filter(item => {
            return item.category === "Angular";
          });
          //CategoryがNode.jsの動画を取得
          self.nodeItems = self.videoItems.filter(item => {
            return item.category === "Node.js";
          });
          //Categoryがotherの動画を取得
          self.otherItems = self.videoItems.filter(item => {
            return item.category === "Other";
          });
          // Javascript初期値フレーム
          self.jsIframe = self.javascriptItems[0].snippet.url;
          
          //jsMap
          self.jsMapitems = self.javascriptItems.map(elm => {
            return {
              url: elm.snippet.url,
              title: elm.snippet.title,
              description: elm.snippet.description,
              thumbnail: elm.snippet.thumbnails.medium.url
            };
          });
          //vueMap
          self.vueMapitems = self.vueItems.map(elm => {
            return {
              url: elm.snippet.url,
              title: elm.snippet.title,
              description: elm.snippet.description,
              thumbnail: elm.snippet.thumbnails.medium.url
            };
          });
          //ReactMap
          self.reactMapitems = self.reactItems.map(elm => {
            return {
              url: elm.snippet.url,
              title: elm.snippet.title,
              description: elm.snippet.description,
              thumbnail: elm.snippet.thumbnails.medium.url
            };
          });
          //AngularMap
          self.angularMapitems = self.angularItems.map(elm => {
            return {
              url: elm.snippet.url,
              title: elm.snippet.title,
              description: elm.snippet.description,
              thumbnail: elm.snippet.thumbnails.medium.url
            };
          });
          //nodeMap
          self.nodeMapitems = self.nodeItems.map(elm => {
            return {
              url: elm.snippet.url,
              title: elm.snippet.title,
              description: elm.snippet.description,
              thumbnail: elm.snippet.thumbnails.medium.url
            };
          });
          //otherMap
          self.otherMapitems = self.otherItems.map(elm => {
            return {
              url: elm.snippet.url,
              title: elm.snippet.title,
              description: elm.snippet.description,
              thumbnail: elm.snippet.thumbnails.medium.url
            };
          });

          console.log(self.otherItems);
          // console.log(self.videoItems);
        });
    });
  },
  methods: {
    //Js動画のvalueを取得
    clickJsItem(value) {
      console.log(value);
      this.jsIframe = value.url;
    },
    //javascriptModal
    javascriptOpenModal() {
      this.jsModal = true;
    },
    javascriptCloseModal() {
      this.jsModal = false;
    },

    //vue動画のvalueを取得
    clickVueItem(value) {
      console.log(value);
      this.vueIframe = value.url;
    },
    //vueModal
    vueOpenModal() {
      this.vueModal = true;
    },
    vueCloseModal() {
      this.vueModal = false;
    },
    //react動画のvalueを取得
    clickReactItem(value) {
      console.log(value);
      this.reactIframe = value.url;
    },
    //reactModal
    reactOpenModal() {
      this.reactModal = true;
    },
    reactCloseModal() {
      this.reactModal = false;
    },
    //angular動画のvalueを取得
    clickAngularItem(value) {
      console.log(value);
      this.angularIframe = value.url;
    },
    //angularModal
    angularOpenModal() {
      this.angularModal = true;
    },
    angularCloseModal() {
      this.angularModal = false;
    },
    //node動画のvalueを取得
    clickNodeItem(value) {
      console.log(value);
      this.nodeIframe = value.url;
    },
    //nodeModal
    nodeOpenModal() {
      this.nodeModal = true;
    },
    nodeCloseModal() {
      this.nodeModal = false;
    },
    //other動画のvalueを取得
    clickOtherItem(value) {
      console.log(value);
      this.otherIframe = value.url;
    },
    //otherModal
    otherOpenModal() {
      this.otherModal = true;
    },
    otherCloseModal() {
      this.otherModal = false;
    },

  }
};
</script>
<style scoped>
#contents {
  margin: 5em 5em;
}
#content {
  padding: 2em 1em;
  box-shadow: 2px 2px 2px 0 rgba(0, 0, 0, 0.2);
  border: 1px solid #eee;
}

h2 {
  padding: 1em;
}
.frames {
  width: 90em;
  height: 50em;
}
.parentItems {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  justify-content: space-between;
}
.childItems {
  width: 32%;
  margin-bottom: 2em;
  box-shadow: 2px 2px 2px 0 rgba(0, 0, 0, 0.2);
  border: 1px solid #eee;
}
.fade-enter {
  transform: translate(-350px, 0);
  opacity: 0;
}
.fade-enter-to {
  opacity: 1;
}
.fade-enter-active {
  transition: all 1s 0.3s ease;
}
.fade-leave {
  transform: translate(0, 0);
  opacity: 1;
}
.fade-leave-to {
  transform: translate(200px, 0);
  opacity: 0;
}
.fade-leave-active {
  transition: all 0.3s 0s ease;
}
</style>