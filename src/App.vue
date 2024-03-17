<template>
  <header>
    <div class="wrapper">
      <nav></nav>
    </div>
  </header>

  <Grain />
  <AnimatedCursor />

  <RouterView />
</template>

<script setup lang="ts">
import { RouterLink, RouterView } from "vue-router";
import Grain from "@/components/Grain.vue";
import AnimatedCursor from "@/components/AnimatedCursor.vue";
</script>

<style lang="scss">
body {
  cursor: none;

  @media (max-width: 768px) {
    cursor: unset;
  }
}

a {
  cursor: pointer;
}

* {
  margin: 0;
  padding: 0;
}

html,
body {
  overflow: auto;
  width: 100%;
  min-height: 100dvh;
}

body::-webkit-scrollbar-track {
  background-color: transparent;
}

body::-webkit-scrollbar {
  width: 10px;
  background-color: transparent;
}

body::-webkit-scrollbar-thumb {
  background-color: rgb(138, 4, 255);
  border: 2px solid rgb(138, 4, 255);
}
</style>
