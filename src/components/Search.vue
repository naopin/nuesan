<template>
  <div class="forms">
    <form action="cgi-bin/example.cgi" method="post">
      <input class="inputkeyword" type="search" name="search" placeholder="キーワードを入力" />
      <input class="inputsearch" type="submit" name="submit" value="検索" />
      <div class="radio">
        <input
          type="radio"
          id="allSearch"
          v-model="keyword"
          value="全て"
          name="KeywordSearch"
          checked="checked"
        />
        <label for="allSearch">全て</label>
        <input type="radio" id="movieSearch" v-model="keyword" value="動画" name="KeywordSearch" />
        <label for="movieSearch">動画</label>
        <input type="radio" id="articleSearch" v-model="keyword" value="記事" name="KeywordSearch" />
        <label for="articleSearch">記事</label>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      keyword: ""
    };
  }
};
</script>
<style scoped>
.forms {
  margin: 0 auto;
  width: 35%;
}
form {
  font-size: 1.8em;
  margin-top: 2em;
}
.inputkeyword {
  width: 85%;
}
.inputsearch {
  width: 15%;
}
.radio {
  text-align: left;
}
</style>