<template>
  <div class="container pt-1">
    <form class="card" @submit.prevent="cratePerson">
      <h2>HTTP WORK</h2>

      <div class="form-control">
        <label for="name">Write your name</label>
        <input type="text" id="name" v-model.trim="name">
      </div>

      <button class="btn primary" :disabled="name.length === 0">Add person</button>

    </form>

    <AppPeopleList 
    :people="people"
    @load="loadPeople"
    /> 
    </div>
</template>

<script>
import AppPeopleList from './components/AppPeopleList.vue'
import axios from 'axios'

export default {
  data() {
    return {
      name: '',
      people: []
    }
  },
  methods: {
    async cratePerson() {
      const response = await fetch('https://vue-http-68bf0-default-rtdb.firebaseio.com/people.json', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          firstName: this.name
        })
      })

      const firebaseData = await response.json()
      this.people.unshift({
        firstName: this.name,
        id: firebaseData.name
      })
      this.name = ''
    },
    async loadPeople() {
      const { data } = await axios.get('https://vue-http-68bf0-default-rtdb.firebaseio.com/people.json')
      const result = Object.keys(data).map(key => {
        return {
          id: key,
          ...data[key]
        }
      })
      this.people = result
    }
  },
  components: { AppPeopleList }
}
</script>

<style lang="scss">
</style>
