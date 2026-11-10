<!-- Vue 2: Options API -->
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

<script>
export default {
  name: 'CounterList',
  props: {
    title: { type: String, required: true }
  },
  data() {
    return { count: 0, items: [] }
  },
  computed: {
    doubled() { return this.count * 2 }
  },
  methods: {
    increment() { this.count++ },
    async fetchItems() {
      const res = await api.get('/items')
      this.items = res.data
    }
  },
  mounted() {
    this.fetchItems()
  }
}
</script>
