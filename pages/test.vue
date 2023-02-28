<script setup lang="ts">
const data = ref('')

await new Promise<void>(resolve => {
  setTimeout(() => {
    data.value = 'Some data'
    resolve()
  }, 1000)
})
</script>

<template>
  <div v-if="data">
    {{ data }}
  </div>
</template>
