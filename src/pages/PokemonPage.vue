<template>

    <h1 v-if="!pokemon">Espere por favor...</h1>

    <div v-else>
        <h1>Quien es este Pokemon?</h1>
        <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
        <PokemonOption 
            :pokemons="pokemonArr"
            @selection="checkAnswer"
        />

        <template v-if="showAnswer" >
            <h2 class="fade-in">{{ message }}</h2>
            <button @click="newGame">
                Nuevo intento
            </button>
        </template>
        
    </div>

</template>

<script>
import PokemonPicture from '@/components/PokemonPicture.vue'
import PokemonOption from '@/components/PokemonOption.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions'


export default {
    name: 'PokemonPage',
    components: {
        PokemonPicture,
        PokemonOption,
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
        async mixiPokemonArray(){
            this.pokemonArr = await getPokemonOptions()
            const rndInt = Math.floor( Math.random() * 4 )
            this.pokemon = this.pokemonArr[ rndInt ]
        },

        checkAnswer(id){
            this.showPokemon = true
            this.showAnswer = true

            if(id === this.pokemon.id){
                this.message = `Correcto - ${ this.pokemon.name }`
            }else{
                this.message = `Incorrecto el pokemon era ${ this.pokemon.name }`
            }
        },
        newGame(){
            this.pokemonArr     = []
            this.showPokemon    = false
            this.showAnswer     = false
            this.pokemon        = null
            this.mixiPokemonArray()
        }
    },
    mounted () {
        this.mixiPokemonArray()
    }
}
</script>
