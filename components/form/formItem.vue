<template>
  <div>
    <label v-if="label">{{ label }}</label>
    <slot></slot>
  </div>
</template>
<script>
export default {
  name: 'iFormItem',
  props: {
    label: { type: String, default: '' },
    prop: { type: String }
  }
}
</script>