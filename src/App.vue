<template>
  <div id="app">
    <div id="nav" class="navbar">
      <router-link class="navbar-brand" to="/">
        <img src="@/assets/logo.svg" id="logo"/>
      </router-link>
      <label for="toggle" id="hamburger">&#9776;</label>
      <input type="checkbox" id="toggle" />
      <ul class="nav">
        <span v-if="isLoggedIn" class="nav-link divider">
          <li class="email">
            <i class="fas fa-user"></i>
            {{" " + currentUser }}
            </li>
        </span>
        <router-link class="nav-link divider" to="/">
          <li class="nav-item">Home</li>
        </router-link>
        <router-link v-if="isLoggedIn" class="nav-link divider" to="/favourites">
          <li class="nav-item">Ulubione</li>
        </router-link>
        <router-link v-if="!isLoggedIn" class="nav-link divider" to="/login">
          <li class="nav-item">Logowanie</li>
        </router-link>
        <router-link v-if="!isLoggedIn" class="nav-link divider" to="/signup">
          <li class="nav-item">Rejestracja</li>
        </router-link>
        <span v-if="isLoggedIn" @click="logout" class="nav-link divider">
          <li id="logout" class="nav-item">Wyloguj</li>
        </span>
        <img alt="logo" src="@/assets/logo.svg" id="logo-bottom" />
      </ul>
    </div>
    <div class="content">
      <router-view />
    </div>
  </div>
</template>

<script>
import { db } from "./firebase";
import { auth } from "./firebase";


export default {
  name: "app",
  
  data() {
    return {
      isLoggedIn: false,
      currentUser: false
    };
  },
  
  firestore() {
    return {
      recipes: db.collection("recipes"),
    };
  },
  
  methods: {
    logout: function() {
      auth.signOut().then( () => {
        this.$router.go({path: this.$router.path})
      })
    }
  },
  created() {
    if(auth.currentUser) {
      this.isLoggedIn = true
      this.currentUser = auth.currentUser.email
    }
  },
  mounted() {
    const links = document.querySelectorAll(".nav-link")
    const logo = document.querySelector(".navbar-brand")

    function hide() {
      let toggle = document.querySelector('#toggle')
      toggle.checked = false
    }

    links.forEach(link => {
      link.onclick = hide
    })
    logo.onclick = hide
  }
};
</script>

<style lang="scss">
@import "src/assets/scss/App.scss";
</style>