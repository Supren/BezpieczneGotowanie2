<template>
  <div class="results-list">
    <FavouritesListItem
      v-for="recipe in favouriteRecipes"
      :id="recipe.recipeId"
      :title="recipe.recipeTitle"
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
import FavouritesListItem from "@/components/FavouritesListItem.vue";
import { auth } from "../firebase";
import { db } from "../firebase";

export default {
  name: "FavouritesList",
  props: ["favouriteRecipes"],
  mounted() {
    let arr = []
    db.collection('usersData').doc(auth.currentUser.uid).collection('favourites').get().then(function(querySnapshot) {
      querySnapshot.forEach(function(doc) {
        arr.push(doc.data())
      })
    })
    this.favouriteRecipes = arr;
    console.log(this.favouriteRecipes)
  },
  components: {
    FavouritesListItem
  }
};
</script>