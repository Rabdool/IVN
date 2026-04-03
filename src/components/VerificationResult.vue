<template>
  <div class="bg-white rounded-3xl border border-gray-100 overflow-hidden shadow-2xl transition-all duration-500 hover:shadow-blue-500/5">
    <!-- Profile Header -->
    <div :class="[data.isGlobal ? 'bg-indigo-600' : 'bg-blue-600']" class="px-8 py-6 text-white flex items-center justify-between transition-colors duration-500">
      <div class="flex items-center gap-3">
        <svg v-if="data.isGlobal" class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9" />
        </svg>
        <svg v-else class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 21V5a2 2 0 00-2-2H7a2 2 0 00-2 2v16m14 0h2m-2 0h-5m-9 0H3m2 0h5M9 7h1m-1 4h1m4-4h1m-1 4h1m-5 10v-5a1 1 0 011-1h2a1 1 0 011 1v5m-4 0h4" />
        </svg>
        <span class="font-black text-sm uppercase tracking-[0.2em]">{{ data.isGlobal ? 'Global Profile Match' : 'National Registry Match' }}</span>
      </div>
      <div class="bg-white/20 px-3 py-1 rounded-full text-[10px] font-bold uppercase tracking-widest backdrop-blur-md">
        Match Confidence: 99.8%
      </div>
    </div>

    <div class="p-8 md:p-12">
      <div class="flex flex-col md:flex-row gap-10">
        <!-- Photo Section -->
        <div class="w-full md:w-1/3">
          <div :class="[data.isGlobal ? 'ring-indigo-100' : 'ring-blue-100']" class="aspect-square rounded-2xl overflow-hidden ring-8 shadow-inner relative group">
            <img :src="data.photo" class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110" />
            <div class="absolute inset-0 bg-gradient-to-t from-black/20 to-transparent"></div>
          </div>
          
          <div class="mt-6 flex flex-col items-center p-4 bg-gray-50 rounded-xl border border-gray-100">
            <p class="text-[10px] font-bold text-gray-400 uppercase tracking-widest mb-1">Country of Origin</p>
            <div class="flex items-center gap-2">
              <span class="text-xl font-bold text-gray-900">{{ data.origin }}</span>
            </div>
          </div>
        </div>

        <!-- Details Section -->
        <div class="w-full md:w-2/3 space-y-8">
          <div>
            <p :class="[data.isGlobal ? 'text-indigo-600' : 'text-blue-600']" class="text-xs font-black uppercase tracking-widest mb-2 transition-colors duration-500">Identified as</p>
            <h3 class="text-4xl md:text-5xl font-black text-gray-900 tracking-tight leading-tight mb-2">{{ data.name }}</h3>
          </div>

          <div class="border-t border-gray-100 pt-8">
            <div class="flex items-center gap-2 mb-4">
              <div :class="[data.isGlobal ? 'bg-indigo-500' : 'bg-blue-500']" class="w-1.5 h-1.5 rounded-full"></div>
              <p class="text-xs font-black text-gray-400 uppercase tracking-widest">About this profile</p>
            </div>
            <p class="text-gray-600 text-lg leading-relaxed font-medium">
              {{ data.about }}
            </p>
          </div>
        </div>
      </div>

      <!-- Footer Action -->
      <div class="mt-12 pt-8 border-t border-gray-50 flex items-center justify-between">
        <p class="text-[10px] font-bold text-gray-300 uppercase tracking-widest">Profile retrieved successfully &bull; Biometric Verification Pass</p>
        <div class="flex gap-4">
          <button @click="$emit('reset')" class="text-sm font-bold text-gray-400 hover:text-blue-600 transition-colors uppercase tracking-widest flex items-center gap-2">
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
            </svg>
            Clear Data
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps(['data'])
defineEmits(['reset'])
</script>
