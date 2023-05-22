<template>
  <div>
    {{ data1 }}
    {{ data2 }}
    <slot />
  </div>
</template>

<script lang="ts" setup>
console.log('layout setup start')

const route = useRoute()
const nuxtApp = useNuxtApp()

const data1 = ref({})
const data2 = ref({})

onServerPrefetch(async () => {
  for (const matchedRoute of route.matched) {
    try {
      //@ts-ignore
      await matchedRoute.components.default.parallelServerPrefetch(nuxtApp)
    } catch (e) {}
  }
})

useAsyncData('data1', async () => {
  console.log('layout query1 start')
  await new Promise(resolve => setTimeout(resolve, 2000))
  console.log('layout query1 end')

  data1.value = { foo1: 123 }
})

useAsyncData('data2', async () => {
  console.log('layout query2 start')
  await new Promise(resolve => setTimeout(resolve, 3000))
  console.log('layout query2 end')

  data2.value = { foo2: 456 }
})

console.log('layout setup end')
</script>
