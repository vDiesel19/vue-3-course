<script setup>
import { ref, onMounted } from 'vue';
const pokemons = ref([]);
const api = 'https://pokeapi.co/api/v2/pokemon';
const ids = [1, 4, 7];

const fetchData = async () => {
  try {
    const responses = await Promise.all(
      ids.map(id => fetch(`${api}/${id}`))
    );
    const data = await Promise.all(
      responses.map(res => res.json())
    );
    pokemons.value = data.map(item => {
      return {
        id: item.id,
        name: item.name,
        sprite: item.sprites.other['official-artwork'].front_default,
        types: item.types.map(types => types.type.name)
      }
    });
  } catch (error) {
    console.error(`fetch error: ${error.message}`)
  }
}

onMounted(() => {
  fetchData();
});
</script>

<template>
  <div class="card" v-for="p in pokemons" :key="p.id">
    <div class="title">
      {{ p.name }}
    </div>
    <div class="content">
      content
    </div>
    <div class="description">
      <div v-for="type in p.types" :key="type">

      </div>
    </div>
  </div>
</template>

<style scoped>
.card {
  border: 1px solid silver;
  border-radius: 8px;
  max-width: 200px;
  margin: 0 5px;
  cursor: pointer;
  box-shadow: 0px 1px 3pm darkgrey;
  transition: 0.2s;
}
.title, 
.content,
.description {
  padding: 16px;
  text-transform: capitalize;
  text-align: center;
}
.title,
.content {
  border-bottom: 1px solid silver;
}
.title {
  font-size: 1.25em;
}
.card:hover {
  transition: 0.2s;
  box-shadow: 0px 1px 9px darkgrey;
}
</style>
