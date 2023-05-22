<template>
  <h3>
    {{ data }}
  </h3>
</template>

<script lang="ts">
import { useNuxtApp } from '#imports'

type NuxtApp = ReturnType<typeof useNuxtApp>

const DATA_KEY = '__someChildData'
const MOCK_DATA = { bar: 14551 }

async function fetchData(nuxtApp: NuxtApp) {
  await new Promise(resolve => setTimeout(resolve, 1000))
  nuxtApp[DATA_KEY] = MOCK_DATA
  return nuxtApp[DATA_KEY]
}

export default defineNuxtComponent({
  name: 'Homepage',

  async setup() {
    console.log('some child setup start')
    const nuxtApp = useNuxtApp()

    const { data } = await useAsyncData('somechild', async () => {
      console.log('some child query start')
      const end = () => console.log('some child query end')

      if (nuxtApp[DATA_KEY]) {
        console.log('using some-child cached data 👍')
        end()
        return nuxtApp[DATA_KEY]
      }
      const data = await fetchData(nuxtApp)
      end()
      return data
    })

    console.log('some child setup end')

    return { data }
  },

  async parallelServerPrefetch(nuxtApp: NuxtApp) {
    console.log('some child prefetch start')
    const data = await fetchData(nuxtApp)
    console.log('some child prefetch end')

    return data
  },
})
</script>
