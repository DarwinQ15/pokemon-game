<template>
    <h1 v-if="!pokemon">Espere por favor...</h1>
    <div v-else>
        <h1>¿Quien es este pokemon?</h1>
        <PokemonImage
            :pokemonId="pokemon.id" 
            :showPokemon="showPokemon" />
        
        <PokemonOptions 
            :pokemons="pokemonArr"
            @selection="checkAnswer"
        />
        <template v-if="showAnswer">
            <h2 class="fade-in">{{ message }}</h2>
            <button @click="newGame">Nuevo juego</button>
        </template>

    </div>
</template>
<script>

import PokemonImage from '@/components/PokemonImage.vue';
import PokemonOptions from '@/components/PokemonOptions.vue';

import getPokemonsOptions from '@/helpers/getPokemonsOptions'

// console.log(getPokemonsOptions());

export default {
    components: {
        PokemonImage,
        PokemonOptions
    },
    data(){
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods: {
        async mixPokemonArray() {
            this.pokemonArr = await getPokemonsOptions()

            const rndInt = Math.floor( Math.random() * 4 )
            this.pokemon = this.pokemonArr[ rndInt ]

            // console.log(this.pokemonArr);
        },
        checkAnswer( selectedId){
            // console.log('pokemon page llamado', pokemonId);
            this.showPokemon = true
            this.showAnswer = true

            if( selectedId=== this.pokemon.id ){
                this.message = `Correcto ${ this.pokemon.name }`
            }else{
                this.message = `Ops, era ${ this.pokemon.name }`
            }

        },
        newGame(){
            this.showPokemon = false
            this.showAnswer = false
            this.pokemonArr = []
            this.pokemon    = null
            this.mixPokemonArray()
        }
    },
    mounted() {
        this.mixPokemonArray()
    }
}
</script>
<style lang="">
    
</style>