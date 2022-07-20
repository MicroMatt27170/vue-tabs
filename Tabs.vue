<template>
  <div :class="classParent">
    <ul role="tablist" :class="tabsClass">
      <li v-for='(item, ind) in tabs'
          role="presentation"
          class="nav-item"
          :key="ind">
        <a role="tab"
           href="#"
           :class="setNavLinkClass(ind)"
           @click="selectTab(ind)"
           v-text="item.title"></a>
      </li>
    </ul>
    <div class="tab-content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  name: "Tabs",
  data() {
    return {
      tabs: [],
      selected: 0
    }
  },
  props: {
    value: {
      default: null,
      type: Number
    },
    pills: {
      default: false,
      type: Boolean
    },
    vertical: {
      default: false,
      type: Boolean
    }
  },
  computed: {
    classParent() {
      return [ this.vertical ? 'd-flex align-items-start' : '' ]
    },
    tabsClass() {
      return [
        'nav',
        this.pills ? 'nav-pills' : 'nav-tabs',
        this.vertical ? 'flex-column p-2' : ''
      ]
    },
    selectedModel: {
      get() {
        return this.value !== null ? this.value : this.selected
      },
      set(s) {
        this.selected = s
        this.$emit('input', s)

        this.changeActive(s)
      }
    },
  },
  watch: {
    value(s) {
      this.changeActive(s)
    }
  },
  mounted() {
    this.setTabs()
    this.changeActive()
  },
  methods: {
    setTabs() {
      this.tabs = this.$slots.default
        .filter(d => d.tag !== undefined)
        .map(d => d.componentInstance)
    },
    setNavLinkClass(index) {
      let tab = this.tabs[index]
      console.log('tab', tab, index)
      return [
        'nav-link',
         this.selectedModel === index ? 'active': '',
        tab.disabled ? 'disabled' : ''
      ]
    },
    changeActive(s = null) {
      s = s !== null ? s : this.selectedModel

      for (let i = 0; i < this.tabs.length; i++) {
        this.tabs[i].setStatus(i === s)
      }
    },
    selectTab(index) {
      this.selectedModel = index
    }
  }
}
</script>

<style scoped>
.nav-link {
  color: var(--theme-primary);
}
.nav-pills .nav-link.active, .nav-pills .show>.nav-link {
  color: var(--theme-on-primary-inverse);
  background-color: var(--theme-primary);
}

</style>
