<template>
  <div class="card" >
    <h3>{{ title }}</h3>
    <button class="btn" @click="toggleSummary()">
        {{ isOpen ? 'Close' : 'Open' }}
    </button>
    <p v-if="isOpen">{{ summary }}</p>
  </div>
</template>

<script>
export default {
  //   props: ['title', 'index', 'summary'],
  props: {
    title: String,
    index: Number,
    summary: String
  },
  data () { 
    return {
      isOpen: false 
    }
  },
  methods: {
    toggleSummary () {   
      this.isOpen = !this.isOpen;
    }
  }
}
</script>

<style lang="scss">
</style>
