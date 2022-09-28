<template>
    <div class="navbar">
      <nav>
        <router-link to="/" exact>Home</router-link>
      </nav>
    </div>
  </template>
  
  <script>
  export default {
    name: 'NavbarComponent',
  }
  </script>
  
  <style scoped>
    a.active{
      background-color: aqua;
    }
  </style>