<template>
  <div class="main-container">
    <UserBalance :user-balance="userBalance"/>
    <GameBar :game-list="gameList"/>
  </div>
</template>

<script setup>
import {onMounted, onDeactivated, ref} from 'vue';
import UserBalance from '../components/UserBalance.vue';
import GameBar from '../components/GameBar.vue';
import router from '../router';

let isAuth = true

const refreshTokenUrl = 'https://poker.evenbetpoker.com/api/web/auth/token?clientId=default'
const gameListUrl = 'https://poker.evenbetpoker.com/api/web/v2/casino/games?clientId=default'

const userBalance = ref(null)
const gameList = ref(null)

onMounted(() => {
  const getRefreshToken = () => {
    console.log('Token ', window.localStorage.getItem('token'))
    console.log('Token refresh ', window.localStorage.getItem('refreshToken'))
    return window.localStorage.getItem('refreshToken')
  }

  const refreshToken = async () => {
    fetch(refreshTokenUrl, {
      method: 'POST',
      mode: 'cors',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        clientId: 'default',
        refreshToken: window.localStorage.getItem('refreshToken')
      })
    })
      .then((response) => {
        return response.json();
      })
      .then(response => {
        const newRefreshToken = response['refresh-token']
        const newToken = response['token']
        window.localStorage.setItem('refreshToken', newRefreshToken)
        window.localStorage.setItem('token', newToken)
      })
      .catch(() => {
        window.localStorage.removeItem('refreshToken')
        window.localStorage.removeItem('token')
        isAuth = false
      }
    )
  }

  if (getRefreshToken()) {
    setInterval(() => {
      refreshToken()
      isAuth = true
    }, 1000 * 800)
  } else {
    isAuth = false
    alert('Вы не авторизованы!')
    router.push('/')
  }
})

onDeactivated(() => {
  window.localStorage.removeItem('token')
  window.localStorage.removeItem('refreshToken')
})

if (isAuth) {
  const getBalance = () => {
    const token = window.localStorage.getItem('token')
    const balanceUrl = `https://poker.evenbetpoker.com/api/web/v2/users/me/balance?clientId=default&auth=${token}`
    fetch(balanceUrl).then(
      response => response.json()
    ).then(response => {
      userBalance.value = response.data[0].attributes
    })
  }
  const getGameList = async () => {
    fetch(gameListUrl).then(
      response => response.json()
    ).then(response => {
      gameList.value = response
    })
  }

  getBalance()
  setInterval(() => {
    getBalance()
  }, 1000 * 30)
  getGameList()
}
</script>

<style lang="sass">
.main-container
  height: 100%
  display: flex
  background: url('../assets/main_image.jpg') no-repeat
  background-size: cover

@media screen and (min-width: 320px)
  .main-container
    flex-direction: column

@media screen and (min-width: 1440px)
  .main-container
    flex-direction: row
</style>
