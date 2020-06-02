<template>
  <div>
    <div class="results-list-item" @click="showRecipeDetails">
      <div class="results-list-item__image">
        <div class="results-list-item__time" v-show="readyInMinutes != null">{{ readyInMinutes }}''</div>

        <img alt="recipe" :src="imageLink" />
      </div>
      <div class="results-list-item__title">{{ title }}</div>
      <!-- <div class="results-list-item__description">Ready in: {{ readyInMinutes }} mins</div> -->
    </div>
    <button
      class="favourites-list-item__info-button"
      @click="deleteFromFavourites"
    >Usuń z ulubionych</button>
  </div>
</template>

<style scoped lang="scss">
@import "src/assets/scss/ResultsListItem.scss";
</style>

<script>
import { db } from "../firebase";
import { auth } from "../firebase";

export default {
  name: "FavouritesListItem",
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
    },
    deleteFromFavourites: function() {
      db.collection("usersData")
        .doc(auth.currentUser.uid)
        .collection("favourites")
        .doc(this.title)
        .delete()
        .then(function() {
          console.log("Deleted");
        })
        .catch(function(error) {
          console.error("Błąd podczas usuwania: ", error);
        });
      this.$router.go({ path: this.$router.path });
    }
  }
};
</script>
