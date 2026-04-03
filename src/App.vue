<template>
  <div class="min-h-screen bg-[#f8fafc] text-[#1e293b] font-sans">
    <!-- Navbar -->
    <header class="bg-white border-b border-gray-200 py-3 px-6 sticky top-0 z-50 shadow-sm">
      <div class="max-w-7xl mx-auto flex items-center justify-between">
        <div class="flex items-center gap-3">
          <div class="bg-blue-600 p-2 rounded-lg">
            <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0zM10 7v3m0 0v3m0-3h3m-3 0H7" />
            </svg>
          </div>
          <h1 class="text-xl font-bold tracking-tight text-blue-900 italic">IVN <span class="text-gray-400 font-normal ml-2 text-sm not-italic uppercase tracking-widest">National Registry Search</span></h1>
        </div>
        <div class="hidden md:flex gap-6 text-sm font-semibold text-gray-500 uppercase tracking-wider">
          <span class="hover:text-blue-600 cursor-pointer">Database</span>
          <span class="hover:text-blue-600 cursor-pointer">Security</span>
          <span class="hover:text-blue-600 cursor-pointer">API Docs</span>
        </div>
      </div>
    </header>

    <main class="max-w-4xl mx-auto px-6 py-12 md:py-20">
      <div v-if="!result && !loading" class="text-center mb-12">
        <h2 class="text-4xl font-extrabold text-blue-950 mb-4 tracking-tight">Identity Reverse Search</h2>
        <p class="text-lg text-gray-500 max-w-xl mx-auto">Upload any portrait to match against the <span class="text-blue-600 font-bold">120M+ Records</span> in the National Registry Database.</p>
      </div>

      <div class="relative">
        <UploadSection v-if="!loading && !result" @verify="handleVerification" />
        
        <div v-if="loading" class="mt-8 transition-all animate-in fade-in zoom-in duration-500">
          <Loader />
        </div>

        <div v-if="result && !loading" class="mt-4 transition-all animate-in slide-in-from-bottom duration-700">
          <div class="flex items-center justify-between mb-6 px-2">
            <div class="flex items-center gap-2 text-sm text-gray-600">
              <span class="bg-green-100 text-green-700 px-2 py-0.5 rounded-full font-bold flex items-center gap-1">
                <span class="w-1.5 h-1.5 bg-green-500 rounded-full animate-pulse"></span>
                1 Best Match Found
              </span>
              <span class="text-gray-300">|</span>
              <span>Search took 3.5s</span>
            </div>
            <button @click="result = null" class="text-sm font-bold text-blue-600 hover:text-blue-800 flex items-center gap-1 group">
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

    <footer class="mt-auto border-t border-gray-100 py-8 px-6 text-center text-sm text-gray-400 font-medium">
      <p>&copy; 2026 National Identity Commission - Advanced Biometric Search System</p>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import UploadSection from './components/UploadSection.vue'
import VerificationResult from './components/VerificationResult.vue'
import Loader from './components/Loader.vue'

const result = ref(null)
const loading = ref(false)

function handleVerification(image) {
  loading.value = true
  result.value = null

  // Simulate AI Analysis and Search Delay
  setTimeout(() => {
    loading.value = false
    // Mock detected ID from image
    result.value = {
      status: 'Verified',
      confidence: '99.2%',
      name: 'His Excellency, President GCFR',
      nin: 'NIG-777-888-001',
      dob: '12 March 2000',
      gender: 'Male',
      state: 'Kano',
      lga: 'Tarauni',
      photo: URL.createObjectURL(image)
    }
  }, 3500)
}
</script>
