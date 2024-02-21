<script lang="ts">
import { defineComponent } from 'vue';

interface ImageData {
  date: string;
  image: string;
  centroid_coordinates: {
    lat: number;
    lon: number;
  };
  caption: string;
}

export default defineComponent({
  data() {
    return {
      dayImages: [] as ImageData[]
    };
  },
  async created() {
    try {
      const res = await fetch('https://api.nasa.gov/EPIC/api/natural/images?api_key=LLWiwai8ZooMBWMdLnNN1nUlLBjtdNRUe9wjH7pA');
      const dayImages: ImageData[] = await res.json();
      this.dayImages = dayImages;
    } catch (error) {
      console.error('Error at get NASA API images:', error);
    }
  },
  methods: {
    getImageUrl(image: ImageData): string {
      const dateString = image.date;
      const dateParts = dateString.split(' ');
      const date = dateParts[0].split('-');
      return `https://api.nasa.gov/EPIC/archive/natural/${date[0]}/${date[1]}/${date[2]}/png/${image.image}.png?api_key=LLWiwai8ZooMBWMdLnNN1nUlLBjtdNRUe9wjH7pA`;
    }
  }
});
</script>

<template>
    <div>
      <div v-for="(image, index) in dayImages" :key="index">
        <img :src="getImageUrl(image)" class="rounded-3xl" :alt="'NASA policromatic Image ' + index">
        <p class="my-4">Coordinates: Latitude: {{ image.centroid_coordinates.lat }}, Longitude: {{ image.centroid_coordinates.lon }}</p>
        <p class="my-4 border-b-2 border-gray-500 mb-72">{{ image.caption }}</p>
      </div>
    </div>
  </template>
  
