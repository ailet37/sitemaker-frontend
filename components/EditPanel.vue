<template>
  <div class="edit-panel">
    <h3>Панель редактирования блока {{ id }}</h3>
    <div class="edit-panel-prop">
      Цвет бордера:
      <input
        :value="color"
        @change="currentColor = $event.target.value"
        type="color"
      />
    </div>
    <button @click="$emit('hide-edit-panel')">Close</button>
    <button @click="$emit('save-data', currentColor)">Save</button>
  </div>
</template>

<style>
.edit-panel {
  margin: 20px;
  border: 1px solid #000;
  padding: 20px;
}

.edit-panel-prop {
  margin-bottom: 20px;
}
</style>

<script>
export default {
  name: 'EditPanel',
  props: ['color', 'id'],
  data() {
    return {
      currentColor: ''
    };
  },
  methods: {
    setColor(value) {
      this.currentColor = value;
    }
  }
};
</script>
