<template>
    <div class="pokemon-finder">
        <input type="text" v-model="searchTerm" placeholder="Type a Pokémon name here...">
        <button @click="searchPokemon">Search</button>

        <div class="pokemon-info" v-if="pokemon.wasFound">
            <ul>
                <li>Name: {{ pokemon.name }}</li>
                <li>Weight: {{ pokemon.weight }}</li>
                <li>Type:
                    <span v-for="type of pokemon.types" :key="type.id">{{type.type.name}} </span>
                </li>
            </ul>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            pokemon: { wasFound: false },
            searchTerm: ''
        }
    },
    methods: {
        searchPokemon() {
            this.pokemon = {}
            this.pokemon.wasFound = false

            axios.get(`https://pokeapi.co/api/v2/pokemon/${this.searchTerm}/`)
                .then((pokemonReturned => {
                    this.pokemon = pokemonReturned.data
                    this.pokemon.wasFound = true;
                }))
                .catch(() => console.log('Error on getting pokemon data.'))
        }
    }
}
</script>

<style>
.pokemon-finder .pokemon-info ul {
    list-style: none;
}
</style>
