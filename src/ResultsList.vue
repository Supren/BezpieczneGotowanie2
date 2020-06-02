<template>
  <div class="results-list">
    <ResultsListItem
      v-for="recipe in receivedRecipes"
      :id="recipe.id"
      :title="recipe.title"
      :readyInMinutes="recipe.readyInMinutes"
      :key="recipe.title"
    />
  </div>
</template>

<style scoped lang="scss">
@import "src/assets/scss/ResultsList.scss";
</style>

<script>
// @ is an alias to /src
import ResultsListItem from "@/components/ResultsListItem.vue";

export default {
  name: "ResultsList",
  props: ["receivedRecipes"],
  mounted() {
    this.receivedRecipes = this.$store.state.results;
  },
  components: {
    ResultsListItem
  }
};
</script>
