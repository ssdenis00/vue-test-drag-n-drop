<template>
  <div>
    <tree :tree-data="org" :drop="drop"></tree>
  </div>
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
                },
                {
                  name: "Отдел 3",
                  children: [{ name: "Otdel 10" }],
                },
                {
                  name: "Отдел 4",
                },
              ],
            },
            {
              name: "Отдел 5",
              children: [
                {
                  name: "Отдел 6",
                },
              ],
            },
            {
              name: "Отдел 7",
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
      const depart = e.dataTransfer.getData("depart");
      const departObj = JSON.parse(depart);
      /* console.log(e.target.textContent, departName); */
      /* console.log(departName); */

      function iterate(obj) {
        for (let key in obj) {
          if (typeof obj[key] == "object") {
            // если значение свойства обьект, вызываем для него функцию iterate(рекурсия)
            iterate(obj[key]);
          } else {
            /* if (obj.name === departName) {
              delete obj[0];
            } */
            if (
              obj.name === e.target.textContent ||
              obj.name === e.target.querySelector(".label").textContent
            ) {
              if (obj.children) {
                obj.children = [...obj.children, departObj];
              } else {
                obj.children = [];
                obj.children = [departObj];
              }
            }
          }
        }
      }

      this.org = this.org.map((depart) => {
        /* console.log(depart); */
        iterate(depart);
        return depart;
      });
      console.log(this.org);
    },
  },
};
</script>
