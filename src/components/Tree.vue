<template>
  <div class="node">
    <div class="row" @click="toggle">
      <div class="status" :class="expanded ? 'expanded' : 'collapsed'"></div>
      <div class="label" :title="displayLabel">{{ displayLabel }}</div>
    </div>
    <div class="children" :class="expanded ? 'expanded' : 'collapsed'">
      <ul>
        <li v-for="(child, index) in children" :key="index">
          <Tree :node="child" :setting="setting"></Tree>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "Tree",
  props: {
    node: {
      type: Object,
      default() {
        return {};
      },
    },
    setting: {
      type: Object,
      default() {
        return {
          lazyload: false,
          load(node, resolve) {
            resolve([]);
          },
        };
      },
    },
  },
  data() {
    return {
      expanded: false,
      label: "anoymouse",
      children: [],

      displayLabel: "",
      loadStatus: 0, // 0: unloaded, 1: loading, 2: loaded
    };
  },
  watch: {
    loadStatus(val) {
      if (this.setting.lazyload && val === 1) {
        this.displayLabel = this.label + " Loading...";
      } else {
        this.displayLabel = this.label;
      }
    },
  },
  created() {
    let { expanded, label, children } = this.node;
    this.expanded = expanded;
    this.label = this.displayLabel = label;
    this.children = children;

    this.loadStatus = children ? 2 : 0;
  },
  methods: {
    toggle() {
      if (this.setting.lazyload && this.loadStatus !== 2) {
        this.loadStatus = 1;
        this.setting.load(this.node, (children) => {
          this.children = children;
          this.loadStatus = 2;
          this.expanded = true;
        });
      } else {
        this.expanded = !this.expanded;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
.node {
  .row,
  .children {
    padding: 5px 20px 5px 20px;
  }
  .row {
    cursor: pointer;
    display: inline-flex;
    align-items: center;
    max-width: 300px;

    &:hover {
      background: rgba(0, 0, 0, 0.05);
    }
    .status {
      margin-right: 5px;
      width: 50px;
      height: 50px;
      background: url("../assets/folder.png") center no-repeat;
      background-size: 50px 50px;
      &.expanded {
        background-image: url("../assets/opened-folder.png");
      }
    }
    .label {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      color: white;
      font-size: 20px;
    }
  }
  .children {
    &.collapsed {
      display: none;
    }
  }
}
</style>
