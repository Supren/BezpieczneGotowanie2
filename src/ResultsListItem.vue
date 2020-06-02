<template>
  <div class="results-list-item" @click="showRecipeDetails">
    <div class="results-list-item__image">
      <div  class="results-list-item__time" v-show="readyInMinutes != null">{{ readyInMinutes }}''</div>
      <img alt="recipe" :src="imageLink" />
    </div>
    <div class="results-list-item__title">{{ title }}</div>
  </div>
</template>

<style scoped lang="scss">
@import "src/assets/scss/ResultsListItem.scss";
</style>

<script>
export default {
  name: "ResultsListItem",
  props: {
    id: Number,
    image: String,
    title: String,
    readyInMinutes: String
  },
  computed: {
    imageLink: function() {
      return `https://spoonacular.com/recipeImages/${this.id}-312x231.jpg`;
    }
  },
  methods: {
    showRecipeDetails: function() {
      this.$store.commit("setRecipeDetailsId", {
        selectedRecipeId: this.id
      });
      this.$router.push("/recipe");
    }
  }
};
</script>
