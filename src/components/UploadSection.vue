<template>
  <div class="relative group">
    <!-- Search Bar Style Container -->
    <div class="bg-white border-2 border-gray-100 rounded-2xl shadow-xl hover:shadow-2xl transition-all p-2 flex flex-col md:flex-row items-center gap-4 group-hover:border-blue-100">
      
      <!-- Upload Zone -->
      <div 
        class="flex-grow w-full border-2 border-dashed border-gray-100 rounded-xl py-6 md:py-10 flex flex-col items-center justify-center cursor-pointer hover:bg-blue-50/30 transition-colors group/zone"
        @dragover.prevent
        @drop.prevent="onDrop"
        @click="triggerFileInput"
      >
        <input type="file" ref="fileInput" accept="image/*" @change="onFileChange" class="hidden" />
        
        <div v-if="!selectedFile" class="flex flex-col items-center text-center px-4">
          <div class="bg-blue-50 p-3 rounded-full mb-3 group-hover/zone:scale-110 transition-transform">
            <svg class="w-8 h-8 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 00-2 2z" />
            </svg>
          </div>
          <p class="text-gray-900 font-bold text-lg mb-1">Drag image here or <span class="text-blue-600 underline underline-offset-4 decoration-2">browse files</span></p>
          <p class="text-gray-400 text-sm">Upload a portrait to identify via National Registry</p>
        </div>

        <div v-else class="flex flex-col items-center text-center px-4">
          <div class="relative mb-3">
            <img :src="previewUrl" class="w-16 h-16 rounded-lg object-cover ring-2 ring-blue-500 shadow-md" />
            <button @click.stop="selectedFile = null" class="absolute -top-2 -right-2 bg-red-500 text-white p-1 rounded-full shadow-lg hover:bg-red-600 transition-colors">
              <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="3" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
          <p class="text-blue-900 font-bold">{{ selectedFile.name }}</p>
          <p class="text-gray-400 text-xs">Ready for registry lookup</p>
        </div>
      </div>

      <!-- Action Button -->
      <button
        @click="emitVerification"
        :disabled="!selectedFile"
        class="w-full md:w-auto px-8 md:px-12 py-5 bg-blue-600 text-white rounded-xl font-bold text-lg hover:bg-blue-700 disabled:bg-gray-200 disabled:text-gray-400 shadow-lg shadow-blue-500/20 active:scale-95 transition-all flex items-center justify-center gap-3 whitespace-nowrap"
      >
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
        </svg>
        Search Identity
      </button>
    </div>

    <!-- Quick Info -->
    <div class="mt-4 flex flex-wrap justify-center gap-6 text-sm text-gray-400 font-medium">
      <div class="flex items-center gap-1.5">
        <svg class="w-4 h-4 text-gray-300" fill="currentColor" viewBox="0 0 20 20">
          <path fill-rule="evenodd" d="M2.166 4.9L10 1.554 17.834 4.9c.427.182.723.633.723 1.14v5.04c0 3.328-2.361 6.17-5.556 7.143L10 19l-3.001-.817C3.806 17.21 1.445 14.368 1.445 11.041v-5.04c0-.507.296-.958.721-1.14zM10 3.062L4.313 5.5v4.54a7.001 7.001 0 004.582 6.517l1.105.301 1.105-.301a7.001 7.001 0 004.582-6.517V5.5L10 3.062z" clip-rule="evenodd" />
        </svg>
        Secure Biometric Transfer
      </div>
      <div class="flex items-center gap-1.5">
        <span class="w-1.5 h-1.5 bg-green-500 rounded-full"></span>
        Database Online
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'

const fileInput = ref(null)
const selectedFile = ref(null)
const previewUrl = ref(null)
const emit = defineEmits(['verify'])

watch(selectedFile, (newVal) => {
  if (newVal) {
    previewUrl.value = URL.createObjectURL(newVal)
  } else {
    previewUrl.value = null
  }
})

function triggerFileInput() {
  fileInput.value.click()
}

function onFileChange(e) {
  const file = e.target.files[0]
  if (file) selectedFile.value = file
}

function onDrop(e) {
  const file = e.dataTransfer.files[0]
  if (file && file.type.startsWith('image/')) {
    selectedFile.value = file
  }
}

function emitVerification() {
  if (selectedFile.value) {
    emit('verify', selectedFile.value)
  }
}
</script>
