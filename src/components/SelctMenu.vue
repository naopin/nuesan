<template>
  <div id="tabBox">
    <div class="tabMenues">
      <div class="tabs" v-for="item in items" :key="item.menu">
        <input type="radio" :value="item.value" :id="item.name" v-model="isActive" />
        <label @click="click(item.value)" :for="item.name">{{item.menu}}</label>
      </div>
    </div>
    <!-- タブメニュークリック時にクリックされたvalueを渡す -->
    <Contents :value="clicked"></Contents>
  </div>
</template>
<script>
import Contents from "../components/Contents";
export default {
  components: { Contents },
  data() {
    return {
      // タブメニューの初期値
      isActive: "JavascriptValue",
      // タブコンテンツの初期値
      clicked: "JavascriptValue",
      items: [
        { menu: "Javascript", value: "JavascriptValue", name: "JavascriptTab" },
        { menu: "Vue.js", value: "VueValue", name: "VueTab" },
        { menu: "React", value: "ReacValue", name: "ReactTab" },
        { menu: "Angular", value: "AngularValue", name: "AngularTab" },
        { menu: "Node.js", value: "NodeValue", name: "NodeTab" },
        { menu: "Other", value: "OtherValue", name: "OtherTab" }
      ]
    };
  },
  methods: {
    click(value) {
      this.clicked = value;
      // console.log(this.clicked)
    }
  }
};
</script>
<style scoped>
.tabs {
  overflow: hidden;
  display: flex;
  box-shadow: 5px 5px 6px -3px rgba(0, 0, 0, 0.5);
  border-radius: 5px;
}
.tabs input {
  display: none;
}
.tabs label {
  text-align: center;
  cursor: pointer;
  padding: 1rem 5rem;
  font-family: "Gabriola", "Skia-Regular_Condensed", "Noteworthy", sans-serif;
  transition: all 0.6s 0s ease;
}
.tabMenues {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2em;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.5);
  background: #e9e9e8;
}
.tabs:nth-child(1) input:checked + label,
.tabs:nth-child(1) label:hover {
  background: rgb(250, 253, 60);
}
.tabs:nth-child(2) input:checked + label,
.tabs:nth-child(2) label:hover {
  background: rgb(116, 255, 60);
}
.tabs:nth-child(3) input:checked + label,
.tabs:nth-child(3) label:hover {
  background: rgb(60, 226, 255);
}
.tabs:nth-child(4) input:checked + label,
.tabs:nth-child(4) label:hover {
  background: rgb(253, 60, 60);
}
.tabs:nth-child(5) input:checked + label,
.tabs:nth-child(5) label:hover {
  background: rgb(60, 253, 108);
  transition: all 0.5s 0s ease;
}
.tabs:nth-child(6) input:checked + label,
.tabs:nth-child(6) label:hover {
  background: rgb(182, 182, 182);
}
</style>