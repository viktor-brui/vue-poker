<template>
  <div class="balance-item">
    <div class="title">Total balance</div>
    <div class="value">{{userBalance?.available}} {{userBalance?.currency}}</div>
  </div>
  <div class="balance-item">
    <div class="title">Bonus</div>
    <div class="value">{{userBalance?.bonus}}</div>
  </div>
  <div class="balance-item">
    <div class="title">In Play</div>
    <div class="value">{{userBalance?.['in-play']}}</div>
  </div>
</template>

<script setup>
  const props = defineProps({
    userBalance: {
      type: Object,
      default: () => {}
    }
  })
</script>

<style lang="sass" scoped>
.balance-item
  display: flex
  width: 100%
  margin: 8px 3px
  padding: 5px
  flex-direction: column
  align-items: center
  border: solid 2px #ffffff
  border-radius: 8px
  background-color: #201e1ed4
  .title
    font-size: 15px
    font-weight: 600
    color: #9b9da4
  .value
    font-size: 18px
    font-weight: 800
    color: #ffffff

@media screen and (min-width: 780px)
  .balance-item
    .title
      font-size: 18px
    .value
      font-size: 22px
</style>
