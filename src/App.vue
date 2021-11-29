<template>
  <div id="app">
    <div class="hero is-white is-gradient is bold">
      <div class="hero-body">
        <h1 class="title">
          <span class="has-text-success">Rick and Morty</span>
          <span class="subtitle"> characters</span>
        </h1>

        <div class="field has-addons is-pulled-rigth">
          <div class="control">
            <input
              v-model="search"
              type="text"
              class="input is-rounded"
              v-on:keyup.enter="searchData"
            />
          </div>
          <div class="control">
            <button
              class="button is-success is-rounded"
              v-on:click="searchData"
            >
              Search
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="container">
      <div
        class="column is-desktop is-mobile is-tablet is-multiverse is-centered"
      >
        <character
          @showModal="showModal"
          v-for="character of characters"
          v-bind:key="character.id"
          v-bind:character="character"
        />
      </div>
      <nav class="pagination" role="navegation" arial-label="pagination">
        <a class="pagination-previous" v-on:click="changePage(page - 1)"
          >Prev</a
        >

        <ul class="pagination-list">
          <li>
            <a class="pagination-link is-current"> {{ page }} </a>
          </li>
        </ul>

        <a class="pagination-next" v-on:click="changePage(page + 1)">Next</a>
      </nav>
    </div>

    <div class="modal" :class="{ 'is-active': modal }" v-if="modal">
      <div class="modal-background" @click="modal = false">
        <div class="modal-card">
          <header class="modal-card-head">
            <p class="modal-card-title">About {{ currentCharacter.name }}</p>
          </header>

          <div class="modal-card-body">
            <p>
              Gender: <strong>{{ currentCharacter.gender }}</strong>
            </p>

            <p>
              Status: <strong>{{ currentCharacter.status }}</strong>
            </p>

            <p>
              Specie: <strong>{{ currentCharacter.species }}</strong>
            </p>

            <p>
              Type: <strong>{{ currentCharacter.type }}</strong>
            </p>
          </div>

          <footer class="modal-card-foot">
            <button class="button" @click="modal = false">Close</button>
          </footer>
        </div>
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
      page: 1,
      pages: 1,
      search: "",
      modal: false,
      currentCharacter: {},
    };
  },
  created() {
    this.fetch();
  },
  methods: {
    fetch() {
      const params = {
        page: this.page,
        name: this.search,
      };

      let result = axios
        .get(" https://rickandmortyapi.com/api/character", { params })
        .then((res) => {
          this.characters = res.data.results;
          this.pages = res.data.info.pages;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    changePage(page) {
      this.page = page <= 0 || page > this.pages ? this.page : page;
      this.fetch();
      //Scroll top
      window.scrollTo(0, 0);
    },
    searchData() {
      this.page = 1;
      this.fetch();
    },
    showModal(id) {
      //id
      // Fetch One
      this.fetchOne(id);
    },
    async fetchOne(id) {
      //HTTP Request
      let result = await axios.get(
        `https://rickandmortyapi.com/api/character/${id}/`
      );
      this.currentCharacter = result.data;
      this.modal = true;

      console.log(this.currentCharacter, "Personaje");
    },
  },
};
</script>

<style>
</style>
