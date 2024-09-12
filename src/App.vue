<template>
  <div class="container pt-1">
    <AppAlert :alert="alert" @close="alert = null"/>
    <form class="card" @submit.prevent="cratePerson">
      <h2>HTTP WORK</h2>

      <div class="form-control">
        <label for="name">Write your name</label>
        <input type="text" id="name" v-model.trim="name">
      </div>

      <button class="btn primary" :disabled="name.length === 0">Add person</button>

    </form>

    <AppLoader v-if="loading"/>

    <AppPeopleList 
    v-else
    :people="people"
    @load="loadPeople"
    @remove="removePerson"
    /> 
    </div>
</template>

<script>
import AppPeopleList from './components/AppPeopleList.vue'
import AppAlert from './components/AppAlert.vue';
import AppLoader from './components/AppLoader.vue';
import axios from 'axios'

export default {
  data() {
    return {
      name: '',
      people: [],
      alert: null,
      loading: false
    }
  },
  mounted() {
    this.loadPeople()
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
      try {
        this.loading = true
        const { data } = await axios.get('https://vue-http-68bf0-default-rtdb.firebaseio.com/people.json')
        if (!data) {
          throw new Error('List is empty')
        }
        this.people = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        this.loading = false
      } catch (e) {
        this.alert = {
          type: 'danger',
          title: 'Error',
          text: e.message
        }
        this.loading = false
      }
    },
    async removePerson(id) {
      try {
        const name = this.people.find(person => person.id === id).firstName
        await axios.delete(`https://vue-http-68bf0-default-rtdb.firebaseio.com/${id}.json`)
        this.people = this.people.filter(person => person.id !== id)
        this.alert = {
          type: 'primary',
          title: 'successful',
          text: `User "${name}" was been deleted`
        }
      } catch (e) {
        this.alert = {
          type: 'danger',
          title: 'Error',
          text: e.message
        }
      }
    }
  },
  components: { AppPeopleList, AppAlert, AppLoader }
}
</script>

<style lang="scss">
</style>
