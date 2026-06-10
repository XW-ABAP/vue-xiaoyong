<script setup>
import DemoGrid from './components/Grid.vue'
import { ref } from 'vue'
const searchQuery = ref('')
const gridColumns = ['name', 'power', 'action']
const gridData = [
  { name: 'Chuck Norris', power: Infinity, action: 'Punch' },
  { name: 'Bruce Lee', power: 9000, action: 'Kick' },
  { name: 'Jackie Chan', power: 7000, action: 'Flip' },
  { name: 'Jet Li', power: 8000, action: 'Spin' }
]

</script>

<template>
  <form id="search">
    Search <input name="query" v-model="searchQuery">
  </form>
  <DemoGrid :data="gridData" :columns="gridColumns" :filter-key="searchQuery">
  </DemoGrid>
</template>
