<template>
    <div v-if="people.length !== 0">
        <div class="card inline" v-for="person in people" :key="person.id">
            <h3>{{ person.firstName }}</h3>
            <button class="btn danger" @click="$emit('remove', person.id)">Delete</button>
        </div>
    </div>
    <div class="card center" v-else>
        <h3>We don't have people here yet! :(</h3>
        <button class="btn" @click="$emit('load')">Load list</button>
    </div> 
</template>

<script>
export default {
  emits: ['load', 'remove'],
  props: ['people', 'idx']
}
</script>

<style scoped>
.inline{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
</style>