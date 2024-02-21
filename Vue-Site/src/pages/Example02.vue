<script lang="ts">
  import { defineComponent } from 'vue';
  
  interface Movie {
    title: string;
    year: string;
    poster: string;
    rank: string;
    actors: string;
  }
  
  export default defineComponent({
    data() {
      return {
        searchTerm: '',
        movies: [] as Movie[]
      };
    },
    methods: {
      async submitForm() {
        const res = await fetch(`https://search.imdbot.workers.dev/?q=${this.searchTerm}`);
        const { description } = await res.json();
  
        const movies = description.map((movie: Record<string, string>) => {
          const {
            '#TITLE': title,
            '#YEAR': year,
            '#IMG_POSTER': poster,
            '#RANK': rank,
            '#ACTORS': actors
          } = movie;
  
          return {
            title,
            year,
            poster,
            rank,
            actors
          } as Movie;
        });
  
        this.movies = movies;
      }
    }
  });
  </script>

<template>
    <div>
      <form class="flex gap-5 mb-10" @submit.prevent="submitForm">
        <input type="text" v-model="searchTerm" placeholder="Avengers, Matrix, ..."  class=" h-10 block w-1/2 text-sm font-medium rounded-2xl text-gray-900 pl-2">
        <button type="submit" class="bg-gray-700 p-2 rounded-2xl hover:bg-sky-600 duration-300">Search</button>
      </form>
  
      <ul v-if="movies.length > 0" class="grid grid-cols-2 gap-2 max-w-2xl mx-auto">
        <li class="mb-4" v-for="(movie, index) in movies" :key="index">
          <article>
            <img
              :src="movie.poster"
              :alt="movie.title"
              class="aspect-[11/16] w-full h-auto object-cover rounded-lg"
            >
            <h2 class="text-white flex justify-between items-center">
              {{ movie.title }}
              <span class="text-xs text-gray-400">{{ movie.year }}</span>
            </h2>
          </article>
        </li>
      </ul>
  
      <p v-else class="text-2xl mb-96">Movies not found.</p>
    </div>
  </template>