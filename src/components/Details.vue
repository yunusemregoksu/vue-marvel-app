<template>
  <div class="col-md-8">
      <div class="details">
        <div class="row">
          <div class="col-md-12">
            <h1>
              {{character.name}} <hr />
            </h1>
          </div>
        </div>
        <div class="row">
          <div class="col-md-5">
            <img
              :src="characterImage"
              :alt="character.name"
            />
          </div>
          <div class="col-md-7">
            <p>{{character.description}}</p>
            <h4>Comics({{character.series.available}})</h4>
            <ul>
              <li v-if="character.comics" :key="comic.resourceURI" v-for="comic in character.comics.items">{{comic.name}}</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  props: {
    character: {
      type: Object,
      required: true
    }
  },
  computed: {
    characterImage() {
      return `${this.character.thumbnail.path}/portrait_uncanny.${
        this.character.thumbnail.extension
      }`;
    }
  }
};
</script>

<style>
.details {
  margin-top: 10px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}
.details img {
  width: 100%;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.details h1 {
  text-align: center;
}
</style>


