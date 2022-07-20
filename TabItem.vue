<template>
  <div role="tabpanel" :class="tabPaneClass">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "TabItem",
  props: {
    title: String,
    disabled: {
      type: Boolean,
      default: false
    },
    padding: {
      default: 'p-2',
      type: String
    }
  },
  computed: {
    tabPaneClass() {
      return [
        'tab-pane',
        this.padding,
        this.isActive ? 'active': '',
        this.disabled ? 'disabled':''
      ]
    }
  },
  data() {
    return {
      isActive: false
    }
  },
  methods: {
    setStatus(isActive = false) {
      this.isActive = isActive
    }
  }
}
</script>

<style scoped>

</style>
