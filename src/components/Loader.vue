<template>
  <div class="flex flex-col items-center justify-center p-12 bg-white rounded-3xl border border-blue-50 shadow-2xl relative overflow-hidden">
    <!-- Registry Search Pulse -->
    <div class="relative w-48 h-48 mb-10 flex items-center justify-center">
      <!-- Outer Rings -->
      <div class="absolute inset-0 border-[1px] border-blue-100 rounded-full animate-[ping_3s_linear_infinite] opacity-50"></div>
      <div class="absolute inset-4 border-[1px] border-blue-200 rounded-full animate-[ping_2s_linear_infinite] opacity-30"></div>
      
      <!-- Main Scanner Container -->
      <div class="absolute inset-8 border-[3px] border-blue-500/20 rounded-full bg-blue-50/30 backdrop-blur-sm shadow-inner flex items-center justify-center overflow-hidden">
        <!-- Scanning Laser -->
        <div class="absolute w-full h-1 bg-gradient-to-r from-transparent via-blue-400 to-transparent shadow-[0_0_20px_rgba(59,130,246,0.8)] z-20 animate-registry-scan"></div>
        
        <!-- Biometric Pattern (Static) -->
        <svg class="w-24 h-24 text-blue-500 opacity-40" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="0.5" d="M9 3v2m6-2v2M9 19v2m6-2v2M5 9H3m2 6H3m18-6h-2m2 6h-2M7 19l-2 2m14-2l2 2M7 5L5 3m14 2l2 2M9 9h6v6H9V9z" />
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="0.5" d="M12 12v.01M12 9v.01M12 15v.01M9 12v.01M15 12v.01" />
        </svg>

        <!-- Scanning Nodes -->
        <div class="absolute inset-0">
          <div v-for="i in 5" :key="i" 
            class="absolute w-1 h-1 bg-blue-400 rounded-full animate-pulse"
            :style="{ top: Math.random()*100 + '%', left: Math.random()*100 + '%' }"
          ></div>
        </div>
      </div>
    </div>

    <!-- Status Text -->
    <div class="space-y-3 text-center">
      <div class="flex items-center justify-center gap-2">
        <span class="flex h-2 w-2 relative">
          <span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-blue-400 opacity-75"></span>
          <span class="relative inline-flex rounded-full h-2 w-2 bg-blue-500"></span>
        </span>
        <p class="text-blue-950 font-black text-xl tracking-tight">{{ currentStatus }}</p>
      </div>
      <div class="flex flex-col items-center gap-1">
        <div class="w-48 h-1 bg-gray-100 rounded-full overflow-hidden">
          <div class="h-full bg-blue-600 animate-[progress_3.5s_ease-in-out]"></div>
        </div>
        <p class="text-gray-400 text-[10px] font-bold uppercase tracking-[0.2em] mt-2">Querying 120M+ Records</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const statuses = [
  'Wait a moment...',
  'Analyzing Profile Features...',
  'Searching Records...',
  'Matching Identity...',
  'Compiling Profile Info...',
  'Finalizing Result...'
]

const currentStatus = ref(statuses[0])
let intervalId = null

onMounted(() => {
  let index = 0
  intervalId = setInterval(() => {
    if (index < statuses.length - 1) {
      index++
      currentStatus.value = statuses[index]
    }
  }, 550)
})

onUnmounted(() => {
  if (intervalId) clearInterval(intervalId)
})
</script>

<style scoped>
@keyframes registry-scan {
  0%, 100% { top: 0%; opacity: 0; }
  5% { opacity: 1; }
  95% { opacity: 1; }
  50% { top: 100%; }
}

@keyframes progress {
  0% { width: 0%; }
  100% { width: 100%; }
}

.animate-registry-scan {
  animation: registry-scan 2s ease-in-out infinite;
}
</style>
