<template>
  <aside class="user-bar">
    <div class="user-balance_container">
      <div class="user-balance_wrapper">
        <BalanceItem :user-balance="props.userBalance"/>
      </div>
    </div>
  </aside>
</template>

<script setup>
import BalanceItem from './BalanceItem.vue';

const props = defineProps({
  userBalance: {
    type: Object,
    default: () => {}
  }
})
</script>

<style lang="sass" scoped>
.user-bar
  min-width: 200px
  height: 100%
  margin: 0 10px
  display: flex
  justify-content: center
  align-items: center
  .user-balance_container
    width: 140px
    margin: 10px 0
    .user-balance_wrapper
      display: flex
      flex-direction: column
      justify-content: center
      align-items: center

@media screen and (min-width: 320px)
  .user-bar
    height: auto
    .user-balance_container
      width: 100%
      overflow-x: auto
      .user-balance_wrapper
        flex-direction: row

@media screen and (min-width: 728px)
  .user-bar
    margin: 0 50px

@media screen and (min-width: 1440px)
  .user-bar
    margin: 0 0 0 50px
    .user-balance_container
      .user-balance_wrapper
        flex-direction: column
</style>
