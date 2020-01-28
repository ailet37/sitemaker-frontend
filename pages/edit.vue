<template>
  <div class="home">
    <Edit-panel
      v-if="currentBlock"
      v-on:hide-edit-panel="hideEditPanel"
      v-on:save-data="onEditBlock"
      :block="currentBlock"
    ></Edit-panel>
    <Block
      v-for="block in blocks"
      :key="block._id"
      :block="block"
      v-on:edit-block="onEditBlock"
    >
      <button @click="showEditPanel(block)">Edit block</button>
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
      color: '',
      currentBlock: null
    };
  },
  created() {
    this.fetchBlocks();
  },
  methods: {
    async fetchBlocks() {
      const { data } = await axios.get(`${server.baseURL}/blocks`);
      this.blocks = data;
    },

    async addNewBlock() {
      const { data } = await axios.post(`${server.baseURL}/block`, {
        title: 'Заголовок',
        type: 'simple',
        color: '#000'
      });
      this.blocks.push(data);
    },

    showEditPanel(block) {
      this.currentBlock = block;
    },

    hideEditPanel() {
      this.currentBlock = null;
    },

    async removeBlock(blockId) {
      const { data } = await axios.delete(
        `${server.baseURL}/blocks/${blockId}`
      );
      this.blocks = data;
    },

    async onEditBlock(blockData) {
      const { data } = await axios.put(
        `${server.baseURL}/blocks/${this.currentBlock._id}`,
        blockData
      );
      const index = this.blocks.findIndex(
        (block) => block._id === this.currentBlock._id
      );
      Vue.set(this.blocks, index, data);
      this.hideEditPanel();
    }
  }
};
</script>

<style>
body {
  text-align: center;
}
</style>
