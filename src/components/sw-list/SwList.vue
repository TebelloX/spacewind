<template>
  <div :class="listStyle.container">
    <slot />
  </div>
</template>
<script>
import SwListItem from '../../themes/default/SwList'
import { findByKey, installComponent } from '../../helpers/utilities'
const { classes, variants } = SwListItem

export default {
  name: 'SwList',
  install(Vue, theme) {
    installComponent(Vue, theme, this)
  },
  props: {
    classes: {
      type: Object,
      default: () => classes
    },
    variants: {
      type: Object,
      default: () => variants
    },
    variant: {
      type: String,
      required: false,
      default: ''
    }
  },
  data() {
    return {
      items: []
    }
  },
  computed: {
    listStyle() {
      this.setChilds()
      let style = findByKey(this.variant, this.variants)

      return { ...this.classes, ...style }
    }
  },
  mounted() {
    this.items = this.$children
    this.setChilds()
  },
  methods: {
    setChilds() {
      this.items.forEach(item => {
        item.classes = this.classes
        item.variants = this.variants
        item.variant = this.variant
        item.isLoading = false
      })
    }
  }
}
</script>
