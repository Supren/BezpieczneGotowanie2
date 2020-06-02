<template>
  <div class="recipe-details">
    <div class="first-row">
      <div class="recipe-details__info">
        <div class="row-title">
          <div>{{ recipeTitle }}</div>
          <div class="i">
            <i
              @click="addToFavourites"
              class="far fa-star recipe-details__addToFav"
              title="Dodaj do ulubionych"
            ></i>
          </div>
        </div>

        <div class="recipe-details__info-general">
          <div class="recipe-details__info-generalRow">
            <div class="recipe-details__info-general-item">
              <div
                class="recipe-details__info-general-item--second-row"
              >{{ ingredientsNumber }} Składników</div>
            </div>
            <div class="recipe-details__info-general-item">
              <div
                class="recipe-details__info-general-item--second-row"
              >{{ readyInMinutes }} Minut</div>
            </div>
          </div>

          <div class="row-title">Składniki:</div>
          <ul class="second-row__list">
            <li class="second-row__list-item" v-for="ingredient in ingredients" :key="ingredient">
              <b>{{ ingredient.name }}</b>
              - {{ ingredient.amount }} {{ ingredient.unit }}
            </li>
          </ul>
        </div>
      </div>
      <div class="recipe-details__image">
        <img :alt="recipeTitle" :src="image" />
      </div>
    </div>

    <hr class="horizontal-line" v-show="instructions != null" />

    <div class="third-row" v-show="instructions != null">
      <div class="row-title">Przygotowanie:</div>
      <div class="third-row__instructions">
        <p>{{ instructions }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
@import "src/assets/scss/RecipeDetails.scss";
</style>

<script>
import { spoonacularApiKey } from "../spoonacular";
import { auth } from "../firebase";
import { db } from "../firebase";

export default {
  name: "RecipeDetails",
  props: ["receivedRecipeDetailsId"],
  data() {
    return {
      recipeTitle: "",
      ingredientsNumber: null,
      readyInMinutes: null,
      calories: null,
      image: "",
      ingredients: [],
      instructions: ""
    };
  },
  methods: {
    addToFavourites: function() {
      db.collection("usersData")
        .doc(auth.currentUser.uid)
        .collection("favourites")
        .doc(this.recipeTitle)
        .set({
          recipeId: this.receivedRecipeDetailsId,
          recipeTitle: this.recipeTitle,
          readyInMinutes: this.readyInMinutes
        });
    }
  },
  mounted() {
    this.receivedRecipeDetailsId = this.$store.state.recipeDetailsId;

    fetch(
      `https://api.spoonacular.com/recipes/${this.receivedRecipeDetailsId}/information?apiKey=${spoonacularApiKey}&includeNutrition=true`
    )
      .then(response => response.json())
      .then(json => {
        this.recipeTitle = json.title;
        this.ingredientsNumber = json.nutrition.ingredients.length;
        this.readyInMinutes = json.readyInMinutes;
        this.calories = Math.trunc(json.nutrition.nutrients[0].amount);
        this.image = json.image;
        this.ingredients = json.nutrition.ingredients;
        this.summary = json.summary;
        this.instructions = json.instructions;
      });
  }
};
</script>
