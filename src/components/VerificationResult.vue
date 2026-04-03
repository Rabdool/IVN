<template>
  <div class="bg-white rounded-2xl border border-gray-200 overflow-hidden shadow-lg">
    <!-- Header -->
    <div :class="[data.isGlobal ? 'bg-indigo-900' : 'bg-blue-900']" class="px-6 py-4 flex items-center justify-between transition-colors duration-500">
      <div class="flex items-center gap-2">
        <div :class="[data.isGlobal ? 'bg-indigo-300' : 'bg-green-400']" class="w-2 h-2 rounded-full animate-pulse"></div>
        <span class="text-white font-bold text-sm uppercase tracking-widest">
          {{ data.isGlobal ? 'Global Passport Match' : 'Official Record Match' }}
        </span>
      </div>
      <div class="text-blue-200 text-xs font-mono">ID: {{ data.isGlobal ? 'INTL' : 'REF' }}-{{ Math.random().toString(36).substr(2, 9).toUpperCase() }}</div>
    </div>

    <div class="p-6 md:p-8">
      <!-- Comparison Grid -->
      <div class="grid grid-cols-1 md:grid-cols-2 gap-8 mb-10">
        <!-- Input Image -->
        <div class="space-y-3">
          <p class="text-xs font-bold text-gray-400 uppercase tracking-tighter">Your Search Image</p>
          <div class="aspect-square rounded-xl overflow-hidden bg-gray-50 border-4 border-gray-100 shadow-inner group relative">
            <img :src="data.photo" class="w-full h-full object-cover grayscale-[0.3]" />
            <div class="absolute inset-0 bg-blue-500/10 mix-blend-overlay"></div>
          </div>
        </div>

        <!-- Registry Match -->
        <div class="space-y-3">
          <p class="text-xs font-bold text-blue-600 uppercase tracking-tighter">
            {{ data.isGlobal ? 'International Best Match' : 'Registry Best Match' }}
          </p>
          <div :class="[data.isGlobal ? 'border-indigo-100 bg-indigo-50' : 'border-blue-100 bg-blue-50']" class="aspect-square rounded-xl overflow-hidden border-4 shadow-md relative group transition-colors duration-500">
            <img :src="data.photo" class="w-full h-full object-cover" />
            <!-- Match Badge -->
            <div class="absolute top-3 right-3 bg-green-500 text-white px-2 py-1 rounded-md text-[10px] font-black shadow-lg">
              {{ data.confidence }} MATCH
            </div>
            <!-- Global Badge -->
            <div v-if="data.isGlobal" class="absolute bottom-3 left-3 bg-indigo-600 text-white px-3 py-1 rounded-full text-[9px] font-bold uppercase tracking-widest shadow-lg flex items-center gap-1.5">
              <svg class="w-3 h-3" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9" />
              </svg>
              Verified Citizen
            </div>
          </div>
        </div>
      </div>

      <!-- Identity Details -->
      <div class="border-t border-gray-100 pt-8">
        <div class="flex flex-col md:flex-row md:items-end justify-between gap-6">
          <div class="space-y-1">
            <h3 class="text-3xl font-black text-blue-950 tracking-tight leading-none">{{ data.name }}</h3>
            <p class="text-blue-600 font-bold tracking-widest text-sm uppercase">
              {{ data.isGlobal ? 'PASSPORT NO:' : 'NIN:' }} {{ data.nin }}
            </p>
          </div>
          <div class="bg-gray-50 px-4 py-2 rounded-lg border border-gray-100">
            <p class="text-[10px] font-bold text-gray-400 uppercase">Verification Status</p>
            <p class="text-green-600 font-black flex items-center gap-1.5">
              <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 20 20">
                <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
              </svg>
              {{ data.status }}
            </p>
          </div>
        </div>

        <div class="mt-8 grid grid-cols-2 md:grid-cols-4 gap-y-8 gap-x-6">
          <div v-if="data.isGlobal" class="space-y-1">
            <p class="text-[10px] font-bold text-gray-400 uppercase tracking-tight">Origin Country</p>
            <p class="text-indigo-600 font-black flex items-center gap-2">
              <span class="w-4 h-3 bg-red-500 rounded-sm"></span> <!-- Mock flag -->
              {{ data.origin }}
            </p>
          </div>
          <div class="space-y-1">
            <p class="text-[10px] font-bold text-gray-400 uppercase tracking-tight">Date of Birth</p>
            <p class="text-blue-900 font-extrabold">{{ data.dob }}</p>
          </div>
          <div class="space-y-1">
            <p class="text-[10px] font-bold text-gray-400 uppercase tracking-tight">Gender</p>
            <p class="text-blue-900 font-extrabold">{{ data.gender }}</p>
          </div>
          <div v-if="!data.isGlobal" class="space-y-1">
            <p class="text-[10px] font-bold text-gray-400 uppercase tracking-tight">Issue Date</p>
            <p class="text-blue-900 font-extrabold">{{ data.issued }}</p>
          </div>
          <div v-if="!data.isGlobal" class="space-y-1">
            <p class="text-[10px] font-bold text-gray-400 uppercase tracking-tight">Expiry Date</p>
            <p class="text-blue-900 font-extrabold">{{ data.expiry }}</p>
          </div>
          <div v-if="!data.isGlobal" class="space-y-1">
            <p class="text-[10px] font-bold text-gray-400 uppercase tracking-tight">State of Origin</p>
            <p class="text-blue-900 font-extrabold">{{ data.state }}</p>
          </div>
          <div v-if="!data.isGlobal" class="space-y-1">
            <p class="text-[10px] font-bold text-gray-400 uppercase tracking-tight">Local Gov Area</p>
            <p class="text-blue-900 font-extrabold">{{ data.lga }}</p>
          </div>
          <div class="space-y-1">
            <p class="text-[10px] font-bold text-gray-400 uppercase tracking-tight">Biometric Status</p>
            <p class="text-green-600 font-black text-xs uppercase tracking-widest flex items-center gap-1">
              <span class="w-1.5 h-1.5 bg-green-500 rounded-full"></span>
              MATCH
            </p>
          </div>
        </div>
      </div>

      <!-- Footer Info -->
      <div class="mt-10 pt-6 border-t border-gray-50 flex flex-wrap gap-4 text-[10px] text-gray-300 font-bold uppercase tracking-widest">
        <span>Biometric Match: Pass</span>
        <span class="text-gray-200">|</span>
        <span>Security Layer: AES-256</span>
        <span class="text-gray-200">|</span>
        <span>Registry Latency: 42ms</span>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps(['data'])
</script>
