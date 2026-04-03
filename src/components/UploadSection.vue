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
          <p class="text-gray-400 text-sm">Upload a portrait to identify via our universal database</p>
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
          <p class="text-gray-400 text-xs">Ready for identification search</p>
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

    <!-- Unified Search Label -->
    <div class="mt-8 flex flex-col items-center justify-center gap-4 px-4">
      <div class="bg-blue-50/50 px-6 py-2 rounded-full border border-blue-100/50 text-blue-600 font-bold text-xs uppercase tracking-[0.25em] animate-pulse">
        Universal Identity Recognition Engine
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
    emit('verify', { image: selectedFile.value })
  }
}
</script>
