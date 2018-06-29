<template>
  <div class="col-md-4">
        <character-list-item v-for="char in characters" :key="char.id"
          :character="char"
          :selected-character="selectedCharacter"
          v-on:CharacterSelected="handleCharacterSelect"
        ></character-list-item>
    </div>
</template>

<script>
import CharacterListItem from "./CharacterListItem.vue";

export default {
  props: ["characters", "selectedCharacter"],
  components: { CharacterListItem },
  methods: {
    handleCharacterSelect(character) {
      this.$emit('CharacterSelected', character);
    },
  }
};
</script>

