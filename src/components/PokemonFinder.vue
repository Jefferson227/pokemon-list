<template>
    <div class="pokemon-finder">
        <input type="text" v-model="searchTerm" @keypress="searchPokemon" placeholder="Type a Pokémon name here...">

        <div class="pokemon-info" v-show="pokemon.wasFound">
            <ul>
                <li>Name: {{ pokemon.name }}</li>
                <li>Weight: {{ pokemon.weight }}</li>
                <li>Type:
                    <span v-for="type of pokemon.types" :key="type.id">{{type.type.name}} </span>
                </li>
            </ul>
        </div>
        <div v-show="!pokemon.wasFound">
            <p>{{ message }}</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data() {
        return {
            pokemon: { wasFound: false },
            searchTerm: '',
            message: 'The Pokémon\'s info should appear here.'
        }
    },
    methods: {
        searchPokemon() {
            this.pokemon = {}
            this.pokemon.wasFound = false
            this.message = "Searching..."

            if (!this.searchTerm.length) return;

            let fnExec = setTimeout(() => {
                clearTimeout(fnExec)

                axios.get(`https://pokeapi.co/api/v2/pokemon/${this.searchTerm}/`)
                    .then((pokemonReturned => {
                        this.pokemon = pokemonReturned.data
                        this.pokemon.wasFound = true;
                    }))
                    .catch(() => this.message = "Pokémon not found.")
            }, 2000)
        }
    }
}
</script>

<style>
.pokemon-finder .pokemon-info ul {
    list-style: none;
}
</style>
