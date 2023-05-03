<script setup>
import { TMDB_API_KEY } from "./key.js";
import { ref } from "vue";
import axios from "axios";

let data = ref(null);
let trailer = ref(null);

async function getData() {
  const id = options.value;
  data.value = (
    await axios.get(
      `https://api.themoviedb.org/3/movie/${id}?api_key=${TMDB_API_KEY}&language=en-US&append_to_response=videos`
    )
  ).data;

  trailer.value = data._rawValue.videos.results.filter((trailer) => {
    return trailer.type === "Trailer";
  });
}
</script>

<template>
  <h1 id="title">Soap2Morrow</h1>
  <div id="all">
    <select id="options">
      <option value="296271">The Devil Conspiracy</option>
      <option value="505642">Black Panther: Wakanda Forever</option>
      <option value="849869">길복순</option>
      <option value="758323">The Pope's Exorcist</option>
      <option value="603692">John Wick: Chapter 4</option>
      <option value="677179">Creed III</option>
      <option value="700391">65</option>
      <option value="980078">Winnie the Pooh: Blood and Honey</option>
      <option value="842945">Supercell</option>
      <option value="804150">Cocaine Bear</option>
    </select>
    <button id="button" @click="getData()">GET</button>
    <div id="content" v-if="(data, trailer)">
      <h1 id="name">{{ data.title }}</h1>
      <div id="double">
        <img
          id="poster"
          :src="`https://image.tmdb.org/t/p/original/${data.poster_path}`"
        />
        <div id="info">
          <p id="view">{{ data.overview }}</p>
          <p id="release">Release Date: {{ data.release_date }}</p>
          <p id="tag">{{ data.tagline }}</p>
          <p id="rev">Revenue: ${{ data.revenue }}</p>
          <p id="pop">Popularity: {{ data.popularity }}</p>
          <p id="runtime">Runtime: {{ data.runtime }}mins</p>
          <p id="vote">Rating: {{ data.vote_average }}</p>
        </div>
        <div id="vid">
          <iframe
            width="400rem"
            height="400rem"
            :src="`https://www.youtube.com/embed/${trailer.at(0).key}`"
            allowfullscreen
          ></iframe>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
* {
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
}

#title {
  text-align: center;
  font-size: larger;
  justify-content: center;
  background-color: black;
  color: antiquewhite;
}

h1 {
  font-size: 3.2em;
  line-height: 1.1;
}

button {
  border-radius: 3px;
  border: 1px;
  padding: 0.1em 1em;
  font-size: 1em;
  font-weight: 500;
  font-family: inherit;
  cursor: pointer;
}

#all {
  margin: 0 auto;
  padding: 2rem;
  text-align: center;
}

#content {
  background-color: rgb(13, 13, 14);
  height: 80vh;
  border: red;
}

#name {
  color: antiquewhite;
}

#poster {
  width: 20rem;
  margin-right: 30rem;
  margin-left: 3rem;
}

#double {
  display: flex;
  color: antiquewhite;
  width: 100%;
}

#info {
  display: grid;
  position: relative;
  grid-template-columns: 400px 100px 100px;
  color: antiquewhite;
  right: 25rem;
  gap: 10px;
}

#vid {
  position: relative;
  right: 15rem;
}
</style>
