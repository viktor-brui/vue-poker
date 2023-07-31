<template>
  <div
    class="game-card"
    v-for="item in gameList?.data"
    :key="item.attributes.id"
  >
    <img
      :src="item.attributes.image"
      class="game-card_image"
      alt="game_image"
    >
    <button
      v-if="item.attributes['has-demo'] === true"
      class="game-play_btn"
      @click.prevent="startGame(item.id)"
    >
      Play demo
    </button>
  </div>
</template>

<script setup>
const props = defineProps(['gameList'])
const baseApiUrl = import.meta.env.VITE_API_BASE_URL

const startGame = (gameId) => {
  const fetchParams = {
    method: 'POST',
    mode: 'cors',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      clientId: 'default',
      gameId
    })
  }
  fetch(
    baseApiUrl + `/v2/casino/games/${gameId}/session-demo?clientId=default`,
    fetchParams
  )
  .then(response => response.json())
  .then(data => {
    const gameUrl = data.data[0].attributes['launch-options']['game-url']
    window.open(gameUrl, '_blank')
  })
}
</script>

<style lang="sass" scoped>
.game-card
  width: 200px
  height: 150px
  background-color: cadetblue
  display: flex
  justify-content: center
  align-items: end
  position: relative
  margin-bottom: 10px
  .game-card_image
    width: 100%
    height: 100%
  .game-play_btn
    position: absolute
    margin-bottom: 30px
    background-color: #0d6efdc4
    font-weight: 800
    border-radius: 10px
    width: 140px
    height: 40px
    color: #ffffff
</style>
