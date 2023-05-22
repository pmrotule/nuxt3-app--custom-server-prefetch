<template>
  <h3>
    {{ data }}
  </h3>
</template>

<script lang="ts">
import { useNuxtApp } from '#imports'

type NuxtApp = ReturnType<typeof useNuxtApp>

const DATA_KEY = '__homepageData'
const MOCK_DATA = { bar: 14551 }

async function fetchData(nuxtApp: NuxtApp) {
  await new Promise(resolve => setTimeout(resolve, 1000))
  nuxtApp[DATA_KEY] = MOCK_DATA
  return nuxtApp[DATA_KEY]
}

export default defineNuxtComponent({
  name: 'Homepage',

  async setup() {
    console.log('..PAGE setup start')
    const nuxtApp = useNuxtApp()

    const { data } = await useAsyncData('homepageData', async () => {
      console.log('..PAGE query start')
      const end = () => console.log('..PAGE query end')

      if (nuxtApp[DATA_KEY]) {
        console.log('..using PAGE cached data 👍')
        end()
        return nuxtApp[DATA_KEY]
      }
      const data = await fetchData(nuxtApp)
      end()
      return data
    })

    console.log('..PAGE setup end')

    return { data }
  },

  async parallelServerPrefetch(nuxtApp: NuxtApp) {
    console.log('..PAGE prefetch start')
    const data = await fetchData(nuxtApp)
    console.log('..PAGE prefetch end')

    return data
  },
})
</script>
