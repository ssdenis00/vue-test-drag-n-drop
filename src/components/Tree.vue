<template>
  <ul class="tree-list" @dragover.prevent @dragenter.prevent @drop="drop">
    <node-tree
      v-for="tree in treeData"
      :key="tree.name"
      :node="tree"
      @dragover.prevent
      @dragenter.prevent
    ></node-tree>
  </ul>
</template>

<script>
import NodeTree from "./NodeTree";

export default {
  props: {
    treeData: Array,
    drop: Function,
  },
  components: {
    NodeTree,
  },
};
</script>

<style>
.tree-list ul {
  padding-left: 16px;
  margin: 6px 0;
  box-sizing: border-box;
  display: flex;
  justify-content: space-around;
  width: 100%;
}
</style>
