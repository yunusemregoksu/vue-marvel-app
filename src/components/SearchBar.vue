<template>
  <div class="row">
        <div class="col-md-6 col-md-offset-3 search-bar">
          <div class="input-group">
            <input
              class="form-control input-lg"
              v-model="term"
              @keypress="handleKeyPress"
              placeholder="Karakter ara"
            />
            <span class="input-group-btn">
              <button
                class="btn btn-info input-lg"
                @click="onSearchButtonClick"
              >
                Ara
              </button>
            </span>
          </div>
        </div>
      </div>
</template>

<script>
export default {
  data() {
    return {
      term: ""
    };
  },
  methods: {

    handleKeyPress(event) {
      if (event.key === "Enter") {
        return this.onSearchButtonClick(this.term);
      }
    },

    onSearchButtonClick(){
      return this.$emit("SearchRequested", this.term)
    }
  }
};
</script>


<style>
.search-bar {
  margin: 20px;
  text-align: center;
}

.search-bar input {
  width: 75%;
}
</style>

