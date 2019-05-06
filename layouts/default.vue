<template>
  <div class="container font-sans text-black">
    <div class="flex justify-between items-center">
      <h1><nuxt-link to="/">My Awesome Blog</nuxt-link></h1>
      <nav class="flex">
        <menu><nuxt-link to="/about">About</nuxt-link></menu>
        <menu><nuxt-link to="/team">Team</nuxt-link></menu>
      </nav>
    </div>
    <nuxt />
  </div>
</template>
