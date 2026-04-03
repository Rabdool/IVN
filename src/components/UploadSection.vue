<template>
  <div>
    <label class="block text-sm font-medium text-gray-700 mb-2">Upload Photo or ID</label>
    <input type="file" accept="image/*" @change="onFileChange"
           class="block w-full text-sm text-gray-900 border border-gray-300 rounded-lg cursor-pointer bg-gray-50 focus:outline-none" />

    <button
      @click="emitVerification"
      :disabled="!selectedFile"
      class="mt-4 w-full bg-green-600 text-white py-2 rounded-lg hover:bg-green-700 disabled:bg-gray-300">
      Verify Identity
    </button>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const selectedFile = ref(null)
const emit = defineEmits(['verify'])

function onFileChange(e) {
  selectedFile.value = e.target.files[0]
}

function emitVerification() {
  if (selectedFile.value) emit('verify', selectedFile.value)
}
</script>
