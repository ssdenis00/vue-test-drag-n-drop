<template>
  <tree :tree-data="org" :drop="drop"></tree>
</template>

<script>
import Tree from "./components/Tree";

export default {
  data: () => {
    return {
      org: [
        {
          name: "Руководство",
          children: [
            {
              name: "Отдел 1",
              children: [
                {
                  name: "Отдел 2",
                  children: [],
                },
                {
                  name: "Отдел 3",
                  children: [
                    {
                      name: "Otdel 10",
                      children: [],
                    },
                  ],
                },
                {
                  name: "Отдел 4",
                  children: [],
                },
              ],
            },
            {
              name: "Отдел 5",
              children: [
                {
                  name: "Отдел 6",
                  children: [],
                },
              ],
            },
            {
              name: "Отдел 7",
              children: [],
            },
          ],
        },
      ],
    };
  },
  components: {
    Tree,
  },
  methods: {
    drop(e) {
      const departObj = JSON.parse(e.dataTransfer.getData("depart"));

      const findNodeByName = (tree, searchId) => {
        let findedNode = undefined;

        const recurse = (tree, searchId, path = []) => {
          for (let index = 0; index < tree.length; index++) {
            const node = tree[index];
            if (node.name === searchId) {
              /* console.log("Node finded path –", [...path, index]);
              console.log("Node", node); */
              findedNode = { path, node, index };
            } else if (node?.children?.length) {
              recurse(node.children, searchId, [...path, index, "children"]);
            }
          }
        };

        recurse(tree, searchId);

        return findedNode;
      };

      console.log(e.target.textContent);

      const ejectNode = (tree, nodeName) => {
        const node = findNodeByName(tree, nodeName);
        const nodeParent = node.path.reduce(
          (nodeParent, path) => nodeParent[path],
          tree
        );
        return nodeParent.splice(node.index, 1);
      };

      const movingNode = ejectNode(this.org, departObj.name);
      console.log(movingNode);

      const insertNode = (tree, nodeName) => {
        const node = findNodeByName(tree, nodeName);
        node.node.children.push(movingNode[0]);
      };

      const target =
        e.target.querySelector(".label") === null
          ? e.target.textContent
          : e.target.querySelector(".label").textContent;

      insertNode(this.org, target);
    },
  },
};
</script>
