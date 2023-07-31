<template>
  <aside class="game-bar">
    <div class="game-bar_container">
      <GameCard :game-list="gameList"/>
    </div>
  </aside>
</template>

<script setup>
import GameCard from './GameCard.vue'

const props = defineProps(['gameList'])
</script>

<style lang="sass" scoped>
.game-bar
  width: 100%
  height: 100vh
  margin: 0 10px
  overflow-y: auto

@media screen and (min-width: 320px)
  .game-bar_container
    display: grid
    margin-top: 20px
    justify-content: center
    grid-template-columns: auto

@media screen and (min-width: 460px)
  .game-bar_container
    grid-template-columns: auto auto
    justify-content: space-around

@media screen and (min-width: 768px)
  .game-bar_container
    grid-template-columns: auto auto auto

@media screen and (min-width: 1024px)
  .game-bar_container
    grid-template-columns: auto auto auto auto

@media screen and (min-width: 1440px)
  .game-bar_container
    margin: 150px 50px
    grid-template-columns: auto auto auto auto
</style>
