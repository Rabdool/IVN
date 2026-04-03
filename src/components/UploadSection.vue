<template>
  <div class="space-y-4">
    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">Full Name (as on ID)</label>
      <input type="text" v-model="formData.name" placeholder="John Doe"
             class="block w-full text-sm p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-green-500 focus:border-green-500" />
    </div>

    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">NIN (National Identity Number)</label>
      <input type="text" v-model="formData.nin" placeholder="1234-5678-9012"
             class="block w-full text-sm p-2 text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-green-500 focus:border-green-500" />
    </div>

    <div>
      <label class="block text-sm font-medium text-gray-700 mb-1">Upload Photo or ID</label>
      <input type="file" accept="image/*" @change="onFileChange"
             class="block w-full text-sm text-gray-900 border border-gray-300 rounded-lg cursor-pointer bg-gray-50 focus:outline-none" />
    </div>

    <button
      @click="emitVerification"
      :disabled="!isFormValid"
      class="mt-4 w-full bg-green-600 text-white py-2 rounded-lg hover:bg-green-700 disabled:bg-gray-300 font-semibold transition-colors">
      Verify Identity
    </button>
  </div>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'

const selectedFile = ref(null)
const formData = reactive({
  name: '',
  nin: ''
})

const emit = defineEmits(['verify'])

const isFormValid = computed(() => {
  return formData.name.trim() !== '' && formData.nin.trim() !== '' && selectedFile.value !== null
})

function onFileChange(e) {
  selectedFile.value = e.target.files[0]
}

function emitVerification() {
  if (isFormValid.value) {
    emit('verify', {
      image: selectedFile.value,
      name: formData.name,
      nin: formData.nin
    })
  }
}
</script>
