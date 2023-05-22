<template>
  <h3>
    {{ data }}
  </h3>
</template>

<script setup>
console.log('some child setup start')

const { data } = await useAsyncData('somechild', async () => {
  console.log('some child query start')
  await new Promise(resolve => setTimeout(resolve, 1000))
  console.log('some child query end')
  return { bar: 14551 }
})

console.log('some child setup end')
</script>
