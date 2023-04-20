<template></template>

<script setup>
import { reactive, ref, computed, defineProps } from 'vue'
const props = defineProps({
  details: {
    type: Object,
    default: () => ({})
  }
})
</script>

<style scoped lang="scss"></style>
