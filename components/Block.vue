<template>
  <div :style="style" class="block">
    <h2 :title="block.title">
      {{ block.title }}
    </h2>
    <slot></slot>
  </div>
</template>

<style>
.block {
  height: 200px;
  border: 2px solid #000;
  margin-bottom: 20px;
  padding: 20px;
}
</style>

<script>
export default {
  name: 'Block',
  props: ['block'],
  computed: {
    style() {
      return 'border-color: ' + this.block.color;
    }
  },
  methods: {
    onChange(value) {
      const blockData = { ...this.block };
      blockData.title = value;

      this.$emit('edit-block', blockData);
    }
  }
};
</script>
