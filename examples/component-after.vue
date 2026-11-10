<!-- Vue 3: Composition API + TypeScript -->
<template>
  <div>
    <h1>{{ title }}</h1>
    <p>Count: {{ count }} (doubled: {{ doubled }})</p>
    <button @click="increment">Increment</button>
    <ul>
      <li v-for="item in items" :key="item.id">{{ item.name }}</li>
    </ul>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import type { Item } from '@/types'
import { fetchItems as apiFetchItems } from '@/api/items'

interface Props {
  title: string
}

const props = defineProps<Props>()

const count = ref(0)
const items = ref<Item[]>([])

const doubled = computed(() => count.value * 2)

function increment() {
  count.value++
}

async function fetchItems() {
  const res = await apiFetchItems()
  items.value = res.data
}

onMounted(() => {
  fetchItems()
})
</script>
