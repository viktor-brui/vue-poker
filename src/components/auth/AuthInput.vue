<template>
  <div>
    <input
      @input="$emit('update:modelValue', $event.target.value)"
      :value="modelValue"
      class="form-input"
      type="text"
    >
  </div>
</template>

<script setup>
  const props = defineProps({
    modelValue: {
      type: String,
      default: '',
      required: true
    }
  })

  const emit = defineEmits(['update:modelValue'])
</script>

<style lang="sass" scoped>
  .form-input
    margin-top: 15px
    min-height: 30px
    padding: 10px 15px 10px 30px
    font-size: 22px
    border-radius: 10px
</style>
