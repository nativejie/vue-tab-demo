<template>
  <div  :class="panelCls()">
    <slot></slot>
  </div>
</template>
<script>
export default {
  data() {
    return {
      show: true,
      currentName: this.name
    };
  },
  name: "TabPanel",
  props: {
    label: {
      type: [String, Function],
      default: ""
    },
    name: [String, Number]
  },
  watch: {
    label() {
      this.updateNav();
    },
    name(val) {
      this.currentName = val;
      this.updateNav();
    }
  },
  inject: ["TabsInstance"],
  computed: {},
  methods: {
    updateNav() {
      this.TabsInstance.updateNav();
    },
    panelCls() {
      return ["tab-panel-content", {['panel-active']: this.show === true}];
    }
  },
  mounted() {
    this.updateNav();
  }
};
</script>
<style scoped>
</style>