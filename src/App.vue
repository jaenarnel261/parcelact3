<template>
  <nav>
    <router-link :to="homeLink" class="nav-link" :class="{ 'button': isHomePage }">Home Page</router-link> |
    <router-link :to="shopLink" @click="navigateToShop" class="nav-link" :class="{ 'button': isShopPage }">Shop</router-link>
  </nav>
  <router-view/>
</template>

<script>
export default {
  data() {
    return {
      isLoggedIn: false
    };
  },
  computed: {
    shopLink() {
      return this.isLoggedIn ? '/shop' : '#'; 
    },
    homeLink() {
      return this.$route.path === '/shop' ? '#' : '/'; 
    },
    isHomePage() {
      return this.$route.path === '/';
    },
    isShopPage() {
      return this.$route.path === '/shop';
    }
  },
  methods: {
    navigateToShop(event) {
      if (!this.isLoggedIn) {
        event.preventDefault(); 
        console.log("You need to be logged in to access the Shop.");
      }
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

.nav-link {
  font-weight: bold;
  color: #2c3e50;
  text-decoration: none;
}

.button {
  display: inline-block;
  padding: 10px 20px;
  background-color: orange; /* Change button color to black */
  color: white;
  border-radius: 5px;
  cursor: pointer;
}

nav a.router-link-exact-active {
  color: black;
}
</style>
