<script setup lang="ts">
import Loading from './Loading.vue'

defineProps<{
  ipInfo: {
    ip: string
    city: string
    region: string
    country: string
    timezone: string
    isp: string
  }
  loading: Boolean
}>()
</script>
<template>
  <section
    class="mx-5 bg-white mt-5 rounded-xl z-20 justify-around gap-3 md:gap-5 py-5 md:p-6 grid md:grid-cols-4 relative max-w-5xl md:mx-auto shadow-xl md:text-left"
  >
    <div class="md:border-r-2 border-gray-200 pr-4">
      <h4
        class="text-gray-400 uppercase text-tiny md:text-xs tracking-widest mb-2 md:mb-3 font-medium"
      >
        IP Address
      </h4>
      <Loading v-if="loading" />
      <p v-else class="text-lg md:text-xl font-medium">{{ ipInfo.ip }}</p>
    </div>
    <div class="md:border-r-2 border-gray-200 pr-4">
      <h4
        class="text-gray-400 uppercase text-tiny md:text-xs tracking-widest mb-2 md:mb-3 font-medium"
      >
        Location
      </h4>
      <Loading v-if="loading" />
      <p v-else class="text-lg md:text-xl font-medium">
        {{ ipInfo.city }}, {{ ipInfo.region }}, {{ ipInfo.country }}
      </p>
    </div>
    <div class="md:border-r-2 border-gray-200 pr-4">
      <h4
        class="text-gray-400 uppercase text-tiny md:text-xs tracking-widest mb-2 md:mb-3 font-medium"
      >
        Timezone
      </h4>
      <Loading v-if="loading" />
      <p v-else class="text-lg md:text-xl font-medium">
        UTC {{ ipInfo.timezone }}
      </p>
    </div>
    <div class="pr-4">
      <h4
        class="text-gray-400 uppercase text-tiny md:text-xs tracking-widest mb-2 md:mb-3 font-medium"
      >
        ISP
      </h4>
      <Loading v-if="loading" />
      <p v-else class="text-lg md:text-xl font-medium">{{ ipInfo.isp }}</p>
    </div>
  </section>
</template>
