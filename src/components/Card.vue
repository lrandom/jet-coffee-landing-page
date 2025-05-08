<script setup>
  const props = defineProps({
    item: {
      type: Object,
      required: true
    }
  })
</script>
<template>
                <div 
                     :key="index"
                     class="bg-white rounded-md shadow-premium overflow-hidden transform transition-all duration-300 hover:shadow-elegant hover:-translate-y-2 group animate-fade-in"
                     :style="{ animationDelay: `${index * 0.05}s` }">
                  <div class="p-4 border-b border-gray-100 flex justify-between items-center">
                    <h3 class="text font-semibold text-gray-900 group-hover:text-coffee-primary transition-colors">{{ item.title }}</h3>
                    <span class="text-xs font-medium px-2 py-1 bg-coffee-primary bg-opacity-10 text-coffee-primary rounded-full">{{ activeCategory }}</span>
                  </div>
                  <div class="relative overflow-hidden bg-gray-200" style="height: 300px;">
                    <div class="h-full flex items-center justify-cente p-6 ">
                      <img :src="item.image" :alt="item.title" 
                      class="max-w-full max-h-full object-contain overflow-hidden rounded-md
                      transform transition-transform duration-500 group-hover:scale-105" />
                    </div>
                  </div>

                  <div class="p-6 flex justify-between items-center bg-gray-50">
                    <button class="text-md font-medium px-4
                     py-2 bg-coffee-primary text-white 
                     rounded-md hover:bg-coffee-dark transition-all duration-300
                     shadow-sm hover:shadow-md flex items-center gap-2 transform hover:-translate-y-0.5">
                      <span>Preview</span>
                      <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 5l7 7m0 0l-7 7m7-7H3" />
                      </svg>
                    </button>
                  </div>
                </div>
</template>