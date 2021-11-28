<template>
  <div id="app">
    <div class="hero is-white is-gradient is bold">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-success">R&M</span>
          <span class="subtitle">characters</span>
        </h1>
        <button class="button is-success is-rounded" v-on:click="fetch">
          Check
        </button>
      </div>
    </div>

    <div class="container">
      <div
        class="column is-desktop is-mobile is-tablet is-multiverse is-centered"
      >
        <character
          v-for="character of characters"
          v-bind:key="character.id"
          v-bind:character="character"
        />
      </div>
    </div>
  </div>
</template>

<script>
//libraries
import axios from "axios";

import Character from "./components/Character";

export default {
  name: "App",
  components: {
    Character,
  },
  data: function () {
    return {
      characters: [],
    };
  },
  created() {
    this.fetch();
  },
  methods: {
    fetch() {
      let result = axios
        .get(" https://rickandmortyapi.com/api/character")
        .then((res) => {
          this.characters = res.data.results;
          console.log(res.data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style>
</style>
