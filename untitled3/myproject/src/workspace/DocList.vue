<template>
  <div class="docs">
    <doc-list-item
      v-for="item in docs"
      :docs-item="item"
      :key="item.docID"
      :currentview="currentview"
      :userID="userID"
      :teamID="teamID"
    ></doc-list-item>
  </div>
</template>

<script>
import DocListItem from "./DocListItem";

export default {
  name: "DocList",
  components: {
    DocListItem,
  },
  props: {
    //文档数组
    docs: {
      type: Array,
      default() {
        return [];
      },
    },
    //当前界面
    currentview: {
      type: Number,
      default: 0,
    },
    //当前用户id
    userID: {
      type: Number,
      default: 0,
    },
    //当前团队id
    teamID: {
      type: Number,
      default: 0,
    },
  },
};
</script>

<style scoped>
.docs {
  /* display: flex; 
    flex-wrap: wrap; */
  padding: 5px;
}
</style>
