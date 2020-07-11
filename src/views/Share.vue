<template>
  <div id="share">
    <Header></Header>
    <div class="share">
      <div>
        <h1>動画検索</h1>
      </div>
      <br />
      <input placeholder="キーワードを入力してください" v-model="keyword" />
      <button @click="search_video">検索</button>

      <div v-show="results">
        <iframe
          width="560"
          height="315"
          :src="resultVideo"
          frameborder="0"
          allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
        >></iframe>
        <table>
          <tr>
            <th>
              <font>No</font>
            </th>
            <th>
              <font>Video</font>
            </th>
            <th>
              <font>Contents</font>
            </th>
          </tr>

          <tr v-for="(movie, index) in results" v-bind:key="movie.id.videoId">
            <!-- No -->
            <td>{{ index + 1 }}</td>
            <!-- Video -->
            <td @click="click(movie)">
              <img v-bind:src="movie.snippet.thumbnails.medium.url" />
            </td>
            <!-- titleとdescription -->
            <td>
              <font>
                <b>{{ movie.snippet.title }}</b>
              </font>
              <br />
              {{ movie.snippet.description}}
            </td>
          </tr>
        </table>
      </div>

      <div class="selectmovie">
        <div class="recommend">
          <div>
            <h3>投稿する動画</h3>
            <div class="category">
              <h3>言語</h3>
              <select v-model="choice" @change="selectCategory">
                <option v-for="catregory in categories" :key="catregory.name">{{catregory.name}}</option>
              </select>
            </div>
            <div class="contents">
              <h3>コンテンツ</h3>
              <select>
                <option>動画</option>
                <option value>記事</option>
              </select>
            </div>
            <h4>{{selectMovieTitle}}</h4>
          </div>
          <button @click="share()">投稿</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import * as firebase from "firebase/app";
import "firebase/auth";
import "firebase/firestore";
import Header from "../components/HeaderSignIn";

export default {
  components: { Header },
  data() {
    return {
      categories: [
        { name: "Javascript" },
        { name: "Vue.js" },
        { name: "React" },
        { name: "Angular" },
        { name: "Node.js" },
        { name: "Other" }
      ],
      choice: "",
      selctedCategory: "",
      movieItems: "",
      selectMovieTitle: "",
      selectMovieUrl: "",
      users: "",
      selcted: "",
      resultVideo: null,
      results: null,
      keyword: "",
      order: "viewCount", // リソースを再生回数の多い順に並べます。
      params: {
        q: "", // 検索クエリを指定します。
        part: "snippet",
        type: "video",
        maxResults: "20", // 最大検索数
        key: "AIzaSyAzgmiuC-kMvgkhuhvV4j01F93kU1AGF50"
      }
    };
  },
  props: {
    msg: String
  },
  methods: {
    search_video: function() {
      this.params.q = this.keyword;
      var self = this;
      axios
        .get("https://www.googleapis.com/youtube/v3/search", {
          params: this.params
        })
        .then(function(res) {
          self.results = res.data.items;
          self.resultVideo = `https://www.youtube.com/embed/${self.results[0].id.videoId}`;
          console.log(self.results);
        });
    },
    click: function(value) {
      this.resultVideo = `https://www.youtube.com/embed/${value.id.videoId}`;
      // 選択された全ての動画情報
      this.movieItems = value;
      // 選択された動画タイトル
      this.selectMovieTitle = this.movieItems.snippet.title;
      // 選択された動画URL
      this.selectMovieUrl = `https://www.youtube.com/embed/${this.movieItems.id.videoId}`;

      console.log(this.movieItems);
      //    console.log(this.resultVideo);
    },
    share: function() {
      this.$nextTick(function() {
        const self = this;

        firebase.auth().onAuthStateChanged(function() {
          let db = firebase.firestore();
          const sharesRef = db.collection("shares");
          sharesRef
            .doc(self.movieItems.id.videoId)
            .set({
              category: self.selctedCategory,
              snippet: {
                title: self.movieItems.snippet.title,
                description: self.movieItems.snippet.description,
                url: `https://www.youtube.com/embed/${self.movieItems.id.videoId}`,
                movieId: self.movieItems.id.videoId,
                thumbnails: {
                  medium: {
                    url: self.movieItems.snippet.thumbnails.medium.url
                  }
                }
              }
            })
            .then(() => {
              console.log("Success edit user.");
            })
            .catch(error => {
              console.error("Error edit user: ", error);
            });
        });
      });
    },
    selectCategory(e) {
      this.selctedCategory = e.target.value;
      // console.log(this.selctedCategory);
    }
  },
  click() {}
};
</script>

<style scoped>
.share {
  font-size: 2em;
}
table {
  border: solid 2px #a7a7a7; /*表全体を線で囲う*/
}
table th {
  color: #141414; /*文字色*/
  background: #cccccc; /*背景色*/
  border: dashed 1px #a7a7a7;
}

table td {
  background: #fcfcfc;
  border: dashed 1px #a7a7a7;
}
</style>
