<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movie/tt0409591">
        <img src="https://upload.wikimedia.org/wikipedia/en/6/68/Sunshine_poster.jpg" alt="Sunshine" class="featured-image">
        <div class="detail">
          <h3>Artwork</h3>
          <p>In the not-too-distant future, Earth's dying sun spells the end for humanity. In a last-ditch effort to save the planet, a crew of eight men and women ventures into space with a device that could revive the star. However, an accident, a grave mistake and a distress beacon from a long-lost spaceship throw the crew and its desperate mission into a tailspin.</p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="SearchMovies()" class="search-box">
  <input type="text" id="search" name="search" placeholder="What are you looking for?" v-model="search">
  <input type="submit" id="submit" name="submit" value="Search">
</form>



    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster">
            <div class="type">{{ movie.Type }}</div>
          </div>

          <div class="detail">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>

        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import env from '@/env.js'

export default {
  setup () {
    const search = ref ("");
    const movies = ref([])

    const SearchMovies = () => {
      if (search.value != "") {
       fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
       .then(response => response.json())
       .then(data => {
        movies.value = data.Search;
        search.value = "";
       })
      }
    }

    return {
      search,
      movies,
      SearchMovies
    }
  }
}
</script>

<style lang="scss">
.home {
  padding: 16px;

  .feature-card {
    position: relative;
    margin-bottom: 24px;

    .featured-image {
      display: block;
      width: 100%;
      height: auto;
      object-fit: cover;
      position: relative;
      z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #fff;
        margin-bottom: 16px;
      }

      p {
        color: #fff;
      }
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #fff;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder {
          color: #f3f3f3;
        }

        &:focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #42b883;
        padding: 16px;
        border-radius: 8px;
        color: #fff;
        font-size: 20px;
        text-transform: uppercase;
        transition: 0.4s;

        &:active {
          background-color: #3b8070;
        }
      }
    }
  }

  .movies-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px -8px;

    .movie {
      width: 100%;
      flex: 1 1 100%;
      padding: 16px 8px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            display: block;
            width: 100%;
            height: auto;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42b883;
            color: #fff;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;
          }
        }

        .detail {
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .year {
            color: #aaa;
            font-size: 14px;
          }

          h3 {
            color: #fff;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }
}

@media (min-width: 600px) {
  .home .movies-list .movie {
    width: 50%;
    flex: 1 1 50%;
  }
}

@media (min-width: 900px) {
  .home .movies-list .movie {
    width: 33.33%;
    flex: 1 1 33.33%;
  }
}
</style>