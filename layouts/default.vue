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

route.matched.forEach(matchedRoute => {
  try {
    //@ts-ignore
    matchedRoute.components.default.parallelServerPrefetch(nuxtApp)
  } catch (e) {}
})

const [{ data: data1 }, { data: data2 }] = await Promise.all([
  useAsyncData('data1', async () => {
    console.log('layout query1 start')
    await new Promise(resolve => setTimeout(resolve, 2000))
    console.log('layout query1 end')

    return { foo1: 123 }
  }),
  useAsyncData('data2', async () => {
    console.log('layout query2 start')
    await new Promise(resolve => setTimeout(resolve, 3000))
    console.log('layout query2 end')

    return { foo2: 456 }
  }),
])

console.log('layout setup end')
</script>
