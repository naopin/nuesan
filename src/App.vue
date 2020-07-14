<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
import 'normalize.css'
export default {
  name: 'App'
}
</script>

<style>
html {
 /* ルートのフォントサイズを10pxに設定 */
 font-size: 62.5%;
}

body {
  font-family: "Yu Gothic", "游ゴシック", "YuGothic", "游ゴシック体", sans-serif;
  color: #333;
  line-height: 1.5;
  letter-spacing: 0.1em;
}


*, *:before, *:after {
  -webkit-box-sizing: border-box;
  box-sizing: border-box
 }

h2,h3,h4,h5 {
  margin: 0;
  padding: 0;
}

img {
  width: 100%;
  vertical-align: bottom;
}

figure {
  margin: 0;
}

table {
  border-collapse: collapse;
  border-spacing: 0;
}

ul,li,dl,dt,dd {
  margin:0;
  padding:0;
  list-style: none;
}

a { 
  text-decoration: none; 
  transition: all .6s ease;
}

a:hover { 
  transition: all .6s ease;
}

.content-wrapper {
  width: 95%;
  margin: 0 auto;
  max-width: 1400px;
  margin: 0 auto;
}


</style>
