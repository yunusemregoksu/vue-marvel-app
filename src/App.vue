<template>
  <div id="app">
    <div class="container">
      <loader v-if="isLoading"></loader>
      <template v-else>
        <search-bar v-on:SearchRequested="characterSearch"></search-bar>
        <div class="row">
          <template v-if="searchFoundData">
            <character-list :characters="characters" v-on:CharacterSelected="handleCharacterSelect" :selected-character="selectedCharacter"></character-list>
            <character-details :character="character"></character-details>
          </template>
          <h1 v-else-if="searchCannotFoundData">Karakter bulunamadı...</h1>
          <loader v-else></loader>
        </div>
      </template>
    </div>
  </div>
</template>

<script>
import SearchBar from "./components/SearchBar.vue";
import CharacterList from "./components/CharacterList.vue";
import CharacterDetails from "./components/Details.vue";
import Loader from "./components/Loader.vue";
import md5 from "md5";

const API_URL = "https://gateway.marvel.com:443/v1/public/";
const publicKey = process.env.VUE_APP_MARVEL_PUBLIC_KEY;
const privateKey = process.env.VUE_APP_MARVEL_PRIVATE_KEY;
const ts = "1";
const auth = `ts=${ts}&apikey=${publicKey}&hash=${md5(
  `${ts}${privateKey}${publicKey}`
)}`;

export default {
  name: "app",
  components: {
    SearchBar,
    CharacterList,
    CharacterDetails,
    Loader
  },
  computed: {
    character() {
      return this.selectedCharacter || this.characters[0];
    }
  },
  data() {
    return {
      isLoading: true,
      searchFoundData: false,
      searchCannotFoundData: false,
      characters: null,
      selectedCharacter: null
    };
  },
  created() {
    this.getInitialCharacters();
  },
  methods: {
    getInitialCharacters() {
      fetch(`${API_URL}/characters?${auth}&limit=5`)
        .then(res => {
          return res.json();
        })
        .then(res => {
          this.isLoading = false;
          this.searchFoundData = true;
          this.characters = res.data.results;
        });
    },

    handleCharacterSelect(character) {
      this.selectedCharacter = character;
    },

    characterSearch(term) {
      this.searchFoundData = false;
      this.searchCannotFoundData = false;
      this.selectedCharacter = null;
      this.characters = null;

      if (term === "") {
        this.getInitialCharacters();
      } else {
        fetch(`${API_URL}/characters?${auth}&limit=5&nameStartsWith=${term}`)
          .then(res => res.json())
          .then(res => {
            this.characters = res.data.results;
            if (res.data.results.length) {
              this.searchFoundData = true;
            } else {
              this.searchCannotFoundData = true;
            }
          });
      }
    }
  }
};
</script>

<style lang="scss">
@import "./styles/custom-bootstrap.scss";
@import "../node_modules/bootstrap/scss/bootstrap.scss";
</style>
