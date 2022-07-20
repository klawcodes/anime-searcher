<template>
  <div class="app">
    <header>
      <div class="image-banner">
        <img class="banner" src="./assets/banner.jpg" alt="" />
      </div>
      <h1>THE<strong>ANIME</strong>SEARCHER</h1>

      <form class="search-box" @submit.prevent="HandleSearch">
        <input type="search" class="search-field" placeholder="Naruto Shippuden..." required v-model="search_query" />
      </form>
    </header>

    <main>
      <div class="cards">
        <Card v-for="anime in animelist" :key="anime.mal_id" :anime="anime"></Card>
      </div>
    </main>
  </div>
</template>

<script>
import { ref } from 'vue';
import Card from './components/AnimeCard.vue';

export default {
  name: 'App',
  components: {
    Card,
  },

  setup() {
    const search_query = ref('');
    const animelist = ref([]);

    const HandleSearch = async () => {
      animelist.value = await fetch(`https://api.jikan.moe/v3/search/anime?q=${search_query.value}`)
        .then((res) => res.json())
        .then((data) => data.results);

      console.log(animelist.value);
    };

    return {
      search_query,
      animelist,
      HandleSearch,
    };
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Anton&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;

  font-family: 'Anton', sans-serif;
  background-color: #ece5c7;
}
.image-banner {
  text-align: center;
}

.banner {
  width: 750px;
  height: 275.5px;
}

a {
  text-decoration: none;
}

header {
  padding-top: 30px;
  padding-bottom: 50px;

  h1 {
    color: #888;
    font-size: 70px;
    font-weight: 400;
    text-align: center;
    text-transform: uppercase;
    margin-bottom: 30px;

    strong {
      color: #313131;
    }

    &:hover {
      color: #313131;
    }
  }

  .search-box {
    display: flex;
    justify-content: center;
    padding-left: 30px;
    padding-right: 30px;

    .search-field {
      appearance: none;
      background: none;
      border: none;
      outline: none;

      background-color: #f3f3f3;
      box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);

      display: block;
      width: 100%;
      max-width: 600px;
      padding: 15px;
      border-radius: 8px;

      color: #313131;
      font-size: 20px;
      font-family: 'Poppins', sans-serif;

      transition: 0.4s;

      &::placeholder {
        color: #aaa;
      }

      &:focus,
      &:valid {
        color: #fff;
        background-color: #313131;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
      }
    }
  }
}

main {
  max-width: 1200px;
  margin: 0px auto;
  padding-left: 30px;
  padding-right: 30px;

  .cards {
    display: flex;
    flex-wrap: wrap;
    margin: 0px -8px;
  }
}
</style>
