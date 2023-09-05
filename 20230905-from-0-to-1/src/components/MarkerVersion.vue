<script setup lang="ts">
defineProps<{
  version: string
}>()
</script>

<template>
  <Marker class="text-blue-500" v-html="version" />
</template>