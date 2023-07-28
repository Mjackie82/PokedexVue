<template>
  <div class="main-container">
    <div class="card" v-for="(poke, index) in pokemons" :key="index">
      <p class="id">#{{index+1}}</p>
      <img :src="this.pokemonsImage[index].url" alt="pokemon image">
      <p class="name">{{pokemonsName[index].name}}</p>
      <div class="types">
        <p>{{ this.pokemonTypes[index].type }}</p>
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
        pokemonTypes: [],
        limit: 1010 // LIMIT 1010
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

        const URLNames = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/"

        for(let i = 1;i <= this.limit;i++){
          this.pokemonsImage.push({id: i, url: URLNames + i + '.png'})
        }

        // get types

        const reqURLTypes = await fetch(`https://pokeapi.co/api/v2/pokemon?limit=${this.limit}`)

        const data = await reqURLTypes.json()

        let currentPokemon = data.results

        let typesToAdd = []

        for(let i = 0; i < this.limit; i++){
          const req = await fetch(currentPokemon[i].url)

          const data = await req.json()

          let currentType = data.types

          let typesList = ""

          for(let j = 0; j < currentType.length; j++){
            typesList += `${currentType[j].type.name} `
          }

          typesToAdd.push({id: i, type: typesList})
          
        }
        
        this.pokemonTypes = typesToAdd

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
    min-height: 150px;
    border-radius: 5%;
    text-align: center;
    display: flex;
    flex-direction: column;
    box-shadow: 0 0 5px 1px;
    padding: 10px 0 10px 0;
    
  }

  .id {
    font-weight: bold;
    font-size: 22px;
    display: flex;
    margin-left: 15px;
    
  }
  
  .name {
    font-weight: bold;
    letter-spacing: .8px;
  }

</style>