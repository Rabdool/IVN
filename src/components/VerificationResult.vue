<template>
  <div class="p-4 border rounded-lg bg-green-50">
    <div class="flex items-center gap-4">
      <img :src="data.photo" alt="Uploaded" class="w-24 h-24 rounded-full object-cover border-2 border-green-600" />
      <div>
        <h3 class="text-lg font-bold text-green-700">{{ data.name }}</h3>
        <p class="text-sm text-gray-600">NIN: {{ data.nin }}</p>
        <p class="text-sm text-gray-600">Status: <span class="font-semibold">{{ data.status }}</span></p>
        <p class="text-sm text-gray-600">Confidence: {{ data.confidence }}</p>
      </div>
    </div>

    <div class="mt-3 text-sm text-gray-700">
      <p>DOB: {{ data.dob }}</p>
      <p>Gender: {{ data.gender }}</p>
      <p>State: {{ data.state }}</p>
      <p>LGA: {{ data.lga }}</p>
    </div>
  </div>
</template>

<script setup>
defineProps(['data'])
</script>
