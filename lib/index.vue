<template lang="html">
  <property
    v-if="!!resolvedSchema"
    :schema="resolvedSchema"
    :model-root="modelWrapper.root"
    :model-wrapper="modelWrapper"
    :options="fullOptions"
    model-key="root"
    parent-key=""
    @error="e => $emit('error', e)"
    @change="e => $emit('change', e)"
    @input="e => $emit('input', e)"
  />
</template>

<script>
import jrefs from './utils/json-refs'
import colors from './utils/colors'
import Property from './components/Property.vue'
export default {
  name: 'VJsonschemaForm',
  components: { Property },
  props: ['schema', 'model', 'options'],
  data() {
    return { modelWrapper: { root: this.model } }
  },
  computed: {
    resolvedSchema() {
      return jrefs.resolve(this.schema)
    },
    fullOptions() {
      const httpLib = this.axios || this.$http || this.$axios
      // we use the multi-themes functionality of vuetify2 as a clue of the version
      const vuetifyVersion = !this.$vuetify.theme.themes ? 1 : 2
      // icons font is only variable in vuetify 2
      const iconfont = vuetifyVersion === 1 ? 'md' : ((this.$vuetify.icons && this.$vuetify.icons.iconfont) || 'mdi')
      const icons = {
        md: {
          calendar: 'event',
          info: 'info',
          dropdown: 'arrow_drop_down',
          dropup: 'arrow_drop_up',
          add: 'add',
          reorder: 'reorder',
          delete: 'delete'
        },
        mdi: {
          calendar: 'mdi-calendar',
          info: 'mdi-information',
          dropdown: 'mdi-menu-down',
          dropup: 'mdi-menu-up',
          add: 'mdi-plus',
          reorder: 'mdi-reorder-horizontal',
          delete: 'mdi-delete'
        }
      }[iconfont]
      return Object.assign({}, {
        debug: false,
        httpLib,
        disableAll: false,
        colors,
        autoFoldObjects: false,
        requiredMessage: 'This information is required',
        noDataMessage: 'No matching value found',
        searchMessage: 'Search...',
        vuetifyVersion,
        icons: Object.assign(icons, this.options.icons || {})
      }, this.options)
    }
  }
}
</script>
