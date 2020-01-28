<template>
  <div :style="style" class="block">
    {{ id }}

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
  props: ['color', 'id'],
  computed: {
    style() {
      return 'border-color: ' + this.color;
    }
  }
};
</script>
