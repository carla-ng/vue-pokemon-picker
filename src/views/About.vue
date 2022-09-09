<template>

  <div class="about">

    <div v-if="pokemon" class="pokemon-card p-5 m-auto mt-4 shadow-2xl flex justify-center flex-col items-center">
      <h3 class="text-2xl text-purple-400 capitalize font-bold m-2"> {{pokemon.name}} </h3>

      <div class="flex justify-center">
        <img class="w-40" :src="pokemon.sprites.front_default" alt="Pokemon Front">
        <img class="w-40" :src="pokemon.sprites.back_default" alt="Pokemon Back">
      </div>

      <div class="info-box m-3 text-center">
        <h3 class="text-yellow-400 text-lg font-semibold underline p-1">Type</h3>
        <div v-for="( type, idx ) in pokemon.types" :key="idx">
          <p class="capitalize text-sm text-grey-700">{{type.type.name}}</p>
        </div>
      </div>
      
    </div>

  </div>

</template>

<script>
import { useRoute } from 'vue-router'
import { reactive, toRefs } from 'vue';


export default {

  setup() {

    const route = useRoute()

    const state = reactive({
      pokemon: null
    })

    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
      .then( ( res ) => res.json() )
      .then( ( data ) => {
          console.log(data)
          state.pokemon = data
    })

    return { ...toRefs(state) }

  }

}
</script>

<style>

.about .pokemon-card {
  background-color: rgba(230, 230, 250, 0.7);
  border-radius: 20px;
  width: 90%;
}
@media (min-width: 992px) {
  .about .pokemon-card { width: 40%; }
}

</style>
