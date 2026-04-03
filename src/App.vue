<template>
  <div class="min-h-screen bg-gray-100 flex flex-col items-center justify-center p-6">
    <h1 class="text-3xl font-bold text-green-600 mb-6">IVN - Identity Verifier for Nigeria</h1>
    
    <div class="bg-white shadow-xl rounded-2xl p-6 w-full max-w-lg">
      <UploadSection @verify="handleVerification" />
      
      <div v-if="loading" class="mt-4">
        <Loader />
      </div>

      <div v-if="result && !loading" class="mt-6">
        <VerificationResult :data="result" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import UploadSection from './components/UploadSection.vue'
import VerificationResult from './components/VerificationResult.vue'
import Loader from './components/Loader.vue'

const result = ref(null)
const loading = ref(false)

function handleVerification(data) {
  loading.value = true
  result.value = null

  // Simulate backend verification delay
  setTimeout(() => {
    loading.value = false
    // Mock response data
    result.value = {
      status: 'Verified',
      confidence: '98%',
      name: data.name,
      nin: data.nin,
      dob: '12 March 2000',
      gender: 'Male',
      state: 'Kano',
      lga: 'Tarauni',
      photo: URL.createObjectURL(data.image)
    }
  }, 2500)
}
</script>
