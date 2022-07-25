<template>
<div class="app">
   <header>
       <h1>Anime<strong>  Search </strong> </h1>
		<form class="search-box" @submit.prevent="HandleSearch">
				<input 
					type="search" 
					class="search-field" 
					placeholder="Search for an anime..."
					required
					v-model="search_query" />
			</form>
  </header>
  <main>
   <div class="cards" v-if="animelist.length > 0">
				<CarDs 
					v-for="anime in animelist" 
					:key="anime.mal_id"
					:anime="anime" />
			</div>
			<div class="no-results" v-else>
        <h3>Soory we have no results ....</h3>
      </div>
  </main>
</div>
</template>

<script>

import { ref } from 'vue';
import CarDs from './components/CarDs.vue';
export default{
    setup() {
      const search_query = ref("");
      const animelist = ref([]);
      const HandleSearch = async () =>{
          animelist.value = await fetch(`https://api.jikan.moe/v3/search/anime?q=${search_query.value}`)
                                         
           .then(res =>res.json())
           .then(data=>data.results);
           console.log(animelist.value);
           search_query.value = "";
      }
        return {
            CarDs,
            search_query,
            animelist,
             HandleSearch
        };
    },
    components: { CarDs }
}
</script>

<style lang="scss">
*{
  margin: 0px;
  padding: 0px;
  box-sizing: border-box;
  font-family: 'Fira Sans', sans-serif;
}
a
{
  text-decoration: none;
}
header
{
  padding-top: 50px;
  padding-bottom: 50px;
  h1{
    color: #888;
    font-size: 42px;
    font-weight: 400;
    text-align: center;
    text-transform: uppercase;
    margin-bottom: 30px;
    strong{
      color: #313131;
    }
    &:hover{
      color:#313131;
    }
  }
}
.search-box {
  display: flex;
  justify-content: center;
  padding-left: 30px;
  padding-right: 30px;
}
   .search-field{
       appearance: none;
       background: none;
       border: none;
       outline: none;
       
       background-color: #f3f3f3;
       box-shadow: 0px 4px 8px rgba(0,0,0,0.15);
        display: block;
       width:100%;
       max-width:  600px;
       padding:  15px;
       border-radius:  8px;
        color:#313131;
        font-size: 20px;
        transition: 0.4s;


        &::placeholder  {
          color:#d45;
        }
        &:focus  {
          color:#FFF;
          background-color: #313131;
          box-shadow: 0px 0px 0px rgba(0,0,0,0.15);
        }
   }
   main
   {
    max-width:  1200px;
    margin: 0 auto;
    padding-left: 30px;
    padding-right: 30px;
    .cards
    {
      display: flex;
      flex-wrap: wrap;
      margin: 0 -8px;
    }
   }
   .no-results h3{
    font-size: 20px;
    color: #d45; 
   }
</style>
