<template>
  <div class="flex max-w-screen-lg w-full justify-around mb-10">
    <a :href="'#/'" :class="{ active: currentPath === '/' }">Home</a> |
    <a :href="'#/example01'" :class="{ active: currentPath === '/example01' }">Example 01</a> |
    <a :href="'#/example02'" :class="{ active: currentPath === '/example02' }">Example 02</a> |
    <a :href="'#/example03'" :class="{ active: currentPath === '/example03' }">Example 03</a> |
    <a :href="'#/example04'" :class="{ active: currentPath === '/example04' }">Example 04</a> |
    <a :href="'#/example05'" :class="{ active: currentPath === '/example05' }">Example 05</a>
  </div>
  <div class="max-w-screen-lg w-full">
    <component :is="currentView"/>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import Home from '../pages/Home.vue'
import Example01 from '../pages/Example01.vue'
import Example02 from '../pages/Example02.vue'
import Example03 from '../pages/Example03.vue'
import Example04 from '../pages/Example04.vue'
import Example05 from '../pages/Example05.vue'
import NotFound from '../pages/NotFound.vue'

interface RouteMap {
  [key: string]: typeof Home | typeof Example01 | typeof Example02 | typeof Example03 | typeof Example04 | typeof Example05 | typeof NotFound
}

const routes: RouteMap = {
  '/': Home,
  '/example01': Example01,
  '/example02': Example02,
  '/example03': Example03,
  '/example04': Example04,
  '/example05': Example05
}

const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] || NotFound
})
</script>

<style scoped>
.active {
  font-weight: bold; /* Cambia el estilo del enlace activo según tus necesidades */
  color: red; /* Por ejemplo, cambia el color a rojo */
}
</style>
