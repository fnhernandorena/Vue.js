 <script lang="ts">
  import { defineComponent } from 'vue';
  
  interface MarsPhoto {
    id: string;
    img_src: string;
    earth_date: string;
  }
  
  export default defineComponent({
    data() {
      return {
        searchTerm: '',
        photos: [] as MarsPhoto[]
      };
    },
    methods: {
      async searchPhotos() {
        try {
          const res = await fetch(`https://api.nasa.gov/mars-photos/api/v1/rovers/curiosity/photos?sol=${this.searchTerm}&page=1&api_key=LLWiwai8ZooMBWMdLnNN1nUlLBjtdNRUe9wjH7pA`);
          const data = await res.json();
  
          this.photos = data.photos;
        } catch (error) {
          console.error('Error at get Mars photos', error);
        }
      }
    }
  });
  </script>

<template>
    <div>
      <form class="flex gap-5 mb-10" @submit.prevent="searchPhotos">
        <input type="text" v-model="searchTerm" placeholder="Write the sun number" class=" h-10 block w-1/2 text-sm font-medium rounded-2xl text-gray-900 pl-2">
        <button type="submit" class="bg-gray-700 p-2 rounded-2xl hover:bg-sky-600 duration-300">Search</button>
      </form>
  
      <ul>
        <li v-for="(image, index) in photos" :key="index">
          <article>
            <img
              :src="image.img_src"
              :alt="image.id"
              class="aspect-[11/16] w-full h-auto object-cover rounded-lg"
            >
            <h2 class="text-white flex justify-between items-center">{{ image.earth_date }}</h2>
          </article>
        </li>
      </ul>
    </div>
  </template>
  
 