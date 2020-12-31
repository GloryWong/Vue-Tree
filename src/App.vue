<template>
  <Tree :node="node" :setting="setting"></Tree>
</template>

<script>
import Tree from "./components/Tree";
import treeTestData from "./test/tree.json";

export default {
  name: "App",
  components: {
    Tree,
  },
  data() {
    return {
      node: {
        expanded: true,
        label: "树的试验",
        children: [
          {
            expanded: false,
            label: "尝试点开我",
          },
          {
            expanded: false,
            label: "没关系的~",
          },
        ],
      },
      setting: {
        lazyload: true,
        load(node, resolve) {
          // Do anything you want here...
          // Async loading
          setTimeout(() => {
            resolve(treeTestData);
          }, 1000);
        },
      },
    };
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
