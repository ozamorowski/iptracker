<script setup lang="ts"></script>

<template>
  <header class="pt-6">
    <div class="container mx-auto">
      <h1 class="text-white text-3xl">IP Address Tracker</h1>
      <input
        type="text"
        class="mt-6 px-6 py-3 w-[35%] rounded-l-2xl focus:shadow focus:outline-none"
        placeholder="Search any IP address or domain"
      />
      <button class="bg-black text-white rounded-r-2xl px-5 py-3 font-bold">
        >
      </button>
    </div>
  </header>
</template>

<style>
#app {
  font-family: 'Rubik', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

header {
  min-height: 250px;
  background-image: url('assets/pattern-bg.png');
}
</style>
