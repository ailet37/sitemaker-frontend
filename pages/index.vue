<template>
  <div class="home">
    <Block v-for="block in blocks" :key="block._id" :block="block"></Block>
    <a href="/edit">edit</a>
  </div>
</template>

<script>
import axios from 'axios';
import Block from '~/components/Block.vue';
import { server } from '~/axios.config.js';

export default {
  components: {
    Block
  },

  data() {
    return {
      blocks: []
    };
  },

  async asyncData({ params }) {
    const { data } = await axios.get(`${server.baseURL}/blocks`);
    return { blocks: data };
  }
};
</script>

<style>
body {
  text-align: center;
}
</style>
