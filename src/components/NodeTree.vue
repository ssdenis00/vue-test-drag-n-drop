<template>
  <li
    class="node-tree"
    draggable="true"
    @dragstart.stop="startDrag($event, node)"
  >
    <span class="label">{{ node.name }}</span>

    <ul v-if="node.children && node.children.length">
      <node
        v-for="child in node.children"
        :node="child"
        :key="child.label"
      ></node>
    </ul>
  </li>
</template>

<script>
export default {
  name: "node",
  props: {
    node: Object,
  },
  methods: {
    startDrag: (e, item) => {
      const itemStr = JSON.stringify(item);
      e.dataTransfer.dropEffect = "move";
      e.dataTransfer.effectAllowed = "move";
      e.dataTransfer.setData("depart", itemStr);
    },
  },
};
</script>

<style scoped>
.node-tree {
  cursor: pointer;
  padding: 10px;
  margin: 15px;
  box-sizing: border-box;
  border: 1px solid black;
}
</style>
