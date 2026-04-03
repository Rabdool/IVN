<template>
  <div class="min-h-screen bg-gray-50 font-sans text-gray-900 selection:bg-blue-100 pb-20">
    <!-- Navigation Header -->
    <header class="bg-white border-b border-gray-100 sticky top-0 z-50 backdrop-blur-md bg-white/80">
      <div class="max-w-7xl mx-auto px-4 h-16 flex items-center justify-between">
        <div class="flex items-center gap-2 group cursor-pointer">
          <div class="w-8 h-8 bg-blue-600 rounded-lg flex items-center justify-center text-white shadow-lg shadow-blue-200 group-hover:scale-110 transition-transform">
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9" />
            </svg>
          </div>
          <h1 class="font-black text-xl tracking-tighter text-blue-900 uppercase">Image Identity <span class="text-blue-600">Verifier</span></h1>
        </div>
      </div>
    </header>

    <main class="max-w-4xl mx-auto px-4 mt-20">
      <!-- Title Section -->
      <div v-if="!loading && !result" class="text-center mb-16 space-y-4">
        <div class="inline-block px-4 py-1.5 bg-blue-50 text-blue-600 rounded-full text-xs font-black uppercase tracking-widest mb-2">
          AI-Powered Visual Recognition
        </div>
        <h2 class="text-5xl md:text-6xl font-black text-gray-900 tracking-tight leading-tight">
          Identify Anyone, <br/>
          <span class="text-blue-600">Instantly.</span>
        </h2>
        <p class="text-gray-400 text-lg font-medium max-w-xl mx-auto">
          Upload an image to discover the identity associated with any portrait using our universal identity recognition engine.
        </p>
      </div>

      <div class="relative">
        <UploadSection v-if="!loading && !result" @verify="searchIdentity" />
        <Loader v-if="loading" />
        
        <div v-if="result && !loading">
          <div class="flex items-center justify-between mb-8 px-2">
            <div class="flex items-center gap-2 text-sm text-gray-400 font-bold uppercase tracking-widest">
              <span class="w-2 h-2 bg-green-500 rounded-full animate-pulse"></span>
              Discovery Complete
            </div>
            <button @click="result = null" class="text-sm font-black text-blue-600 hover:text-blue-800 flex items-center gap-2 group transition-colors">
              <svg class="w-4 h-4 group-hover:-translate-x-1 transition-transform" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18" />
              </svg>
              New Search
            </button>
          </div>
          <VerificationResult :data="result" @reset="result = null" />
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import UploadSection from './components/UploadSection.vue'
import Loader from './components/Loader.vue'
import VerificationResult from './components/VerificationResult.vue'

const result = ref(null)
const loading = ref(false)

function searchIdentity({ image }) {
  loading.value = true
  result.value = null

  // Simulate Universal Search Delay
  setTimeout(() => {
    loading.value = false
    
    // Extract a "Name" from the filename for a generic simulation
    const fileName = image.name.split('.')[0]
    const formattedName = fileName
      .replace(/[_-]/g, ' ')
      .replace(/\b\w/g, l => l.toUpperCase()) || 'Unknown Subject'

    // Generic Profile Response
    result.value = {
      name: formattedName,
      origin: 'Global Citizen',
      about: `This profile has been identified within the universal biometric database. The system has confirmed a match with high confidence, recognizing the subject based on globally indexed public records and professional registry data.`,
      photo: URL.createObjectURL(image)
    }
  }, 2800)
}
</script>
