<template>
    <div v-if="people.length !== 0">
        <div class="card" v-for="person in people" :key="person.id">
            <h3>{{ person.firstName }}</h3>
        </div>
    </div>
    <div class="card center" v-else>
        <h3>We don't have people here yet! :(</h3>
        <button class="btn" @click="$emit('load')">Load list</button>
    </div> 
</template>

<script>
export default {
  emits: ['load'],
  props: ['people', 'idx']
}
</script>

<style lang="scss" scoped>

</style>