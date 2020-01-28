<template>
  <div class="home">
    <Edit-panel
      v-show="showPanel"
      v-on:hide-edit-panel="hideEditPanel"
      v-on:save-data="onEditBlock"
      :id="currentBlockId"
    ></Edit-panel>
    <Block
      v-for="block in blocks"
      :key="block._id"
      :id="block._id"
      :color="block.color"
    >
      <button @click="showEditPanel(block._id)">Edit block</button>
      <button @click="removeBlock(block._id)">Delete block</button>
    </Block>
    <button @click="addNewBlock">Add new block</button>
    <a href="/">render</a>
  </div>
</template>

<script>
import Vue from 'vue';
import axios from 'axios';
import Block from '~/components/Block.vue';
import EditPanel from '~/components/EditPanel.vue';
import { server } from '~/axios.config.js';

export default {
  components: {
    Block,
    EditPanel
  },
  data() {
    return {
      blocks: [],
      showPanel: false,
      color: '',
      currentBlockId: null
    };
  },
  created() {
    this.fetchBlocks();
  },
  methods: {
    renderSite() {},

    fetchBlocks() {
      axios
        .get(`${server.baseURL}/blocks`)
        .then((data) => (this.blocks = data.data));
    },

    addNewBlock() {
      axios
        .post(`${server.baseURL}/block`, { type: 'simple', color: '#000' })
        .then((data) => {
          this.blocks.push(data.data);
        });
    },

    showEditPanel(id) {
      this.showPanel = true;
      this.currentBlockId = id;
    },

    hideEditPanel() {
      this.showPanel = false;
      this.currentBlockId = null;
    },

    removeBlock(blockId) {
      axios.delete(`${server.baseURL}/blocks/${blockId}`).then((data) => {
        window.location.reload();
      });
    },

    onEditBlock(color) {
      const postData = {
        type: 'simple',
        color
      };
      axios
        .put(`${server.baseURL}/blocks/${this.currentBlockId}`, postData)
        .then((data) => {
          const index = this.blocks.findIndex(
            (block) => block._id === this.currentBlockId
          );
          Vue.set(this.blocks, index, data.data);
          this.hideEditPanel();
        });
    }
  }
};
</script>

<style>
body {
  text-align: center;
}
</style>
