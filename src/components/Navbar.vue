<template>
  <nav class="navbar">
    <router-link to="/">Home</router-link>
    <router-link to="/catalog">Catalogo</router-link>
  </nav>
</template>

<script>
  export default{
    name: "Navbar"
  }
</script>

<style scoped>
  nav {
    width: 100%;
    height: 5em;
    background-color: orangered;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    gap: 1em;
    padding: 0 1em;
    font-size: 1rem;
  }

  a {
    color: #fff;
    text-decoration: none;
    letter-spacing: .05em;
    font-weight: bold;
    transition: .5s;
  }
</style>