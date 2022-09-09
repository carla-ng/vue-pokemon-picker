<template>

  <div class="w-full flex justify-center">
    <input type="text" placeholder="Enter Pokemon name" class="mt-10 p-2 border-blue-500 border-2 text-center" v-model="text"/>
  </div>

  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div class="ml-4 text-2x text-blue-400" v-for="( pokemon, idx ) in filteredPokemons" :key="idx">

      <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
        {{pokemon.name}}
      </router-link>
      
    </div>
  </div>

</template>

<script>
// @ is an alias to /src
import { reactive, toRefs, computed } from 'vue'

export default {
  name: 'Home',
  
  setup() {

    const state = reactive({
      pokemons: [],
      urlIdLookup: {},
      text: '',
      filteredPokemons: computed( () => updatePokemon() )
    })

    function updatePokemon() {
      if ( !state.text ) {
        return []
      }

      return state.pokemons.filter( ( pokemon ) =>
        pokemon.name.includes(state.text)
      )
    }

    fetch('https://pokeapi.co/api/v2/pokemon?offset=0')
      .then( ( res ) => res.json() )
      .then( ( data ) => {
          state.pokemons = data.results
          state.urlIdLookup = data.results.reduce( ( acc, cur, idx ) => 
            acc = { ...acc, [cur.name]:idx+1 } , {}
          )
    })

    return {
      ...toRefs(state)
    }

  }

}
</script>
