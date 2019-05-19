<template>
    <div>
        <input type="text" placeholder="Type a Pokémon name here...">
        <button @click="searchPokemon">Search</button>
        <div v-if="pokemon.wasFound">
            <ul>
                <span>Name: {{ pokemon.name }}</span>
                <span>Weight: {{ pokemon.weight }}</span>
            </ul>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            pokemon: { wasFound: false }
        }
    },
    methods: {
        searchPokemon() {
            this.pokemon.wasFound = false

            axios.get('https://pokeapi.co/api/v2/pokemon/pikachu/')
                .then((pokemonReturned => {
                    this.pokemon = pokemonReturned.data
                    this.pokemon.wasFound = true
                }))
                .catch(() => console.log('Error on getting pokemon data.'))
        }
    }
}
</script>
