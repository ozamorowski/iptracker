<script setup lang="ts">
import 'leaflet/dist/leaflet.css'
import L from 'leaflet'
import { onMounted, ref } from 'vue'

import IpInfo from './components/IpInfo.vue'

let map: L.Map
let inputQuery = ref('')
let lastMarker = ref(L.marker([0, 0]))
let loading = ref(false)
let error = ref(false)
let ipInfo = ref({
  ip: '',
  isp: '',
  timezone: '',
  city: '',
  region: '',
  country: '',
})

const mapMarkerIcon = L.divIcon({
  html: '<svg xmlns="http://www.w3.org/2000/svg" width="46" height="56"><path fill-rule="evenodd" d="M39.263 7.673c8.897 8.812 8.966 23.168.153 32.065l-.153.153L23 56 6.737 39.89C-2.16 31.079-2.23 16.723 6.584 7.826l.153-.152c9.007-8.922 23.52-8.922 32.526 0zM23 14.435c-5.211 0-9.436 4.185-9.436 9.347S17.79 33.128 23 33.128s9.436-4.184 9.436-9.346S28.21 14.435 23 14.435z"/></svg>',
  iconSize: [24, 40],
  className: '',
})

const getIpInfo = async (): Promise<void> => {
  loading.value = true
  error.value = false

  const baseUrl: string =
    'https://geo.ipify.org/api/v2/country,city?apiKey=at_oC6vXlb4vmhsNipQcHGi24XZq6Kbf'

  const url: string =
    baseUrl + (inputQuery.value ? '&domain=' + inputQuery.value : '')

  try {
    const request = await fetch(url)
    const response = await request.json()

    if (response.code) {
      error.value = true
      return
    }

    const {
      ip,
      isp,
      location: { lat, lng, timezone, city, region, country },
    } = response

    ipInfo.value = { ip, isp, timezone, city, region, country }

    if (lastMarker.value) {
      map.removeLayer(lastMarker.value as unknown as L.Layer)
    }
    lastMarker.value = L.marker([lat, lng], { icon: mapMarkerIcon }).addTo(map)
    map.setView([lat, lng], 13)

    loading.value = false
  } catch (error) {
    console.log(error)
  }
}

onMounted(() => {
  // Zoom animation couse markers to not saving lat and lng, so it's disabled.
  map = L.map('map', { zoomAnimation: false })

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 19,
    attribution: '© OpenStreetMap',
  }).addTo(map)

  getIpInfo()
})
</script>

<template>
  <header class="pt-6 max-h-64">
    <div class="container mx-auto">
      <h1 class="text-white text-3xl font-bold">IP Address Tracker</h1>
      <input
        v-model="inputQuery"
        @keyup.enter.native="getIpInfo"
        type="text"
        class="mt-8 mb-5 px-6 py-3 md:w-[35%] rounded-l-xl focus:shadow focus:outline-none"
        placeholder="Search any IP address or domain"
      />
      <button
        @click="getIpInfo"
        class="bg-black text-white rounded-r-xl px-5 py-3 font-bold"
      >
        >
      </button>

      <span class="block text-red-500" v-if="error">
        Domain or IP not found.
      </span>

      <IpInfo :ipInfo="ipInfo" :loading="loading" />
    </div>
  </header>
  <div id="map" class="z-10"></div>
</template>

<style>
#app {
  font-family: 'Rubik', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

header {
  min-height: 250px;
  background-image: url('assets/pattern-bg.png');
  background-position: center;
  background-size: cover;
}

#map {
  height: 78vh;
}
</style>
