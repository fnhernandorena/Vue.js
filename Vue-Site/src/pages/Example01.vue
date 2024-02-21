<script lang="ts">
import { defineComponent } from 'vue';
import axios from 'axios';

interface Fact {
  fact: string;
}

export default defineComponent({
  data() {
    return {
      fact: null as Fact | null
    };
  },
  methods: {
    async getFact() {
      try {
        const response = await axios.get<Fact>('https://catfact.ninja/fact');
        this.fact = response.data;
      } catch (error) {
        console.error('Error at get Cat Fact', error);
      }
    }
  }
});
</script>

<template>
  <div class="flex flex-col items-center mb-32">
    <button @click="getFact" class="bg-gray-700 hover:bg-sky-500 text-white font-bold py-4 px-8 rounded-xl mb-10">
      Get Cat Fact!
    </button>
    <p v-if="fact" class="text-3xl text-yellow-400">{{ fact.fact }}</p>
    <p v-else class="text-3xl text-red-500">¡Click to get cat fact!</p>
  </div>
</template>