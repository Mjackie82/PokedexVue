<template>
  <div class="main-container">
    <div class="card" v-for="(poke, index) in pokemons" :key="index">
      <img :src="this.pokemonsImage[index].url" alt="">
      <p class="name">{{pokemonsName[index].name}}</p>
      <div class="types">
        <p>tipo pokemon</p>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "PokemonsGrid",
    data(){
      return{
        pokemons: null,
        pokemonsName: null,
        pokemonsImage: [],
        limit: 151
      }
    },
    methods: {
      async getPokemons(){
          const req = await fetch(`https://pokeapi.co/api/v2/pokemon?limit=${this.limit}`);

          const data = await req.json();

          this.pokemons = data.results;

          this.getStats(this.pokemons);
      },
      async getStats(pokemons){
        const pokes = (Array.from(pokemons));

        this.pokemonsName = pokes
        //console.log(this.pokemonsImage+'1'+'.png')

        const URLNames = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/"

        for(let i = 1;i <= this.limit;i++){
          this.pokemonsImage.push({id: i, url: URLNames + i + '.png'})
        }

        // get types

        const URLTypes = await fetch("https://pokeapi.co/api/v2/pokemon/1")

        const types = await URLTypes.json()

        console.log(types.types[1].type.name);


      }
    },
    mounted(){
      this.getPokemons()
    } 
  }
</script>

<style scoped>
  .main-container {
    width: 100%;
    height: 100vh;
    padding: 1.5em;
    display: flex;
    flex-wrap: wrap;
    gap: .8em;
    justify-content: center;
    align-content: flex-start;
  }

  .card {
    min-width: 150px;
    width: 10%;
    min-height: 100px;
    height: 200px;
    border-radius: 5%;
    text-align: center;
    display: flex;
    flex-direction: column;
    box-shadow: 0 0 5px 1px;

  }

  
  
</style>