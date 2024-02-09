<script setup lang="ts">
import Chart from './components/Chart.vue'
import { useQuery } from '@tanstack/vue-query'
const URL = "https://rest.statica.pl/rest/stockquotes/gpw:PLKGHM000017?type=trades&dt_from=2010-01-01&limit=10000"

const fetcher = async (): Promise<any[]> => {
  const LOGIN = "frontend2024"
  const PASS = "test"
  try {
    const response = await fetch(URL, {
      headers: {
        "Authorization": `Basic ${btoa(LOGIN + ":" + PASS)}`
      }
    })
    const data = await response.json()
    console.log({data})
    return data
  } catch {
    throw new Error('Oh no!')
  }
}
type Data = {
  amount: number;
  dt: number;
  price: number;
}
const { isLoading, isFetching, isError, data, error } = useQuery<Data[]>({
  queryKey: ['daata'],
  queryFn: fetcher,
})
// console.log({isLoading, data})
const xValues = Array.isArray(data) && data?.map(d => d.dt)
// console.log(xValues, data)

</script>

<template>
  {{ console.log(isLoading, data?.[0]) }}
  <div>
    <div v-if="isLoading">Loading...</div>
    <div v-else-if="isError">An error has occurred: {{ error }}</div>
    <div v-else-if="isFetching">Getting data...</div>
    <div v-else-if="data.length">{{ xValues }}</div>
    <div v-else>No data</div>
  </div>
</template>

<style scoped>

</style>
