<script setup>
// Component logic can be added here
import { ref, computed } from 'vue';
import Card from './components/Card.vue';
import Logo from './components/Logo.vue';

// Function to dynamically import images
const getImageUrl = (imagePath) => {
  // If the path starts with a slash, prepend the base URL
  if (imagePath.startsWith('/')) {
    return import.meta.env.BASE_URL.replace(/\/$/, '') + imagePath;
  }
  try {
    // For images in the assets directory
    return new URL(`./assets/${imagePath}`, import.meta.url).href;
  } catch (error) {
    console.warn(`Failed to load image from assets: ${imagePath}`, error);
    // Fallback to public directory
    return import.meta.env.BASE_URL.replace(/\/$/, '') + '/' + imagePath.replace(/^\//, '');
  }
};

// State to track active category and loading state
const activeCategory = ref('dashboard');
const isLoading = ref(false);
const searchQuery = ref('');
const initialItemsToShow = 6; // Initial number of items to show
const displayLimit = ref(initialItemsToShow);

// Function to change active category with loading state
const setActiveCategory = (category) => {
  if (category === activeCategory.value) return;
  
  isLoading.value = true;
  searchQuery.value = ''; // Reset search when changing category
  displayLimit.value = initialItemsToShow; // Reset display limit when changing category
  
  setTimeout(() => {
    activeCategory.value = category;
    isLoading.value = false;
  }, 300); // Short timeout for smoother transition
};

// Function to load more items
const loadMore = () => {
  displayLimit.value += initialItemsToShow;
};

// Content for each category
const categoryContent = {
  dashboard: [
    { title: 'Analytics', image: getImageUrl('/img/sc/light/dashboard/analytics.png') },
    { title: 'CRM', image: getImageUrl('/img/sc/light/dashboard/crm.png') },
    { title: 'Project', image: getImageUrl('/img/sc/light/dashboard/project.png') },
    { title: 'Ecommerce', image: getImageUrl('/img/sc/light/dashboard/ecommerce.png') }
  ],
  ecommerce: [
    { title: 'Product Listing', image: getImageUrl('/img/sc/light/ecommerce/8.png') },
    { title: 'Product Details', image: getImageUrl('/img/sc/light/ecommerce/7.png') },
    { title: 'Shopping Cart', image: getImageUrl('/img/sc/light/ecommerce/5.png') },
    { title: 'Checkout Page', image: getImageUrl('/img/sc/light/ecommerce/6.png') },
    { title: 'Customer List', image: getImageUrl('/img/sc/light/ecommerce/4.png') },
    { title: 'Order List', image: getImageUrl('/img/sc/light/ecommerce/2.png') },
    { title: 'Checkout Page', image: getImageUrl('/img/sc/light/ecommerce/3.png') }
  ],
  components: [
    { title: 'Accordion', image: getImageUrl('/img/sc/light/components/1.png') },
    { title: 'Alert', image: getImageUrl('/img/sc/light/components/2.png') },
    { title: 'Avatar', image: getImageUrl('/img/sc/light/components/3.png') },
    { title: 'Button', image: getImageUrl('/img/sc/light/components/4.png') },
    { title: 'Badge', image: getImageUrl('/img/sc/light/components/5.png') },
    { title: 'Chip', image: getImageUrl('/img/sc/light/components/6.png') },
    { title: 'Modal', image: getImageUrl('/img/sc/light/components/7.png') },
    { title: 'List', image: getImageUrl('/img/sc/light/components/8.png') },
    { title: 'Pagination', image: getImageUrl('/img/sc/light/components/9.png') },
    { title: 'Progress', image: getImageUrl('/img/sc/light/components/10.png') },
    { title: 'Tab', image: getImageUrl('/img/sc/light/components/11.png') },
    { title: 'Tooltip', image: getImageUrl('/img/sc/light/components/12.png') },
    { title: 'Dropdown', image: getImageUrl('/img/sc/light/components/13.png') },
  ],
  apps: [
    { title: 'Kanban Board', image: getImageUrl('/img/sc/light/apps/kanban.png') },
    { title: 'Chat', image: getImageUrl('/img/sc/light/apps/chat.png') },
    { title: 'Email', image: getImageUrl('/img/sc/light/apps/email.png') },
    { title: 'Todo', image: getImageUrl('/img/sc/light/apps/todo.png') },
    { title: 'File Manager', image: getImageUrl('/img/sc/light/apps/file_manager.png') }
  ],
  auth: [
    { title: 'Login Page Basic', image: getImageUrl('/img/sc/light/auth/1.png') },
    { title: 'Login Page Cover', image: getImageUrl('/img/sc/light/auth/2.png') },
    { title: 'Signup Page Basic', image: getImageUrl('/img/sc/light/auth/3.png') },
    { title: 'Signup Page Cover', image: getImageUrl('/img/sc/light/auth/4.png') },
    { title: 'Verify Email', image: getImageUrl('/img/sc/light/auth/5.png') },
    { title: 'Verify Email With Cover', image: getImageUrl('/img/sc/light/auth/6.png') },
    { title: 'Reset Password', image: getImageUrl('/img/sc/light/auth/7.png') },
    { title: 'Reset Password With Cover', image: getImageUrl('/img/sc/light/auth/8.png') },
  ]
};

// Filtered content based on search query
const filteredContent = computed(() => {
  if (!searchQuery.value) return categoryContent[activeCategory.value];
  
  const query = searchQuery.value.toLowerCase();
  return categoryContent[activeCategory.value].filter(item => 
    item.title.toLowerCase().includes(query)
  );
});

// Limited content based on display limit
const displayedContent = computed(() => {
  // If searching, show all filtered results
  if (searchQuery.value) return filteredContent.value;
  
  // Show all items instead of limiting
  return filteredContent.value;
});

// Gallery section
const activeGalleryCategory = ref('dashboard');
const gallerySearchQuery = ref('');
const isGalleryLoading = ref(false);
const galleryCategories = {
  dashboard: [
    { title: 'Analytics', image: getImageUrl('/img/sc/dark/dashboard/1.png') },
    { title: 'CRM', image: getImageUrl('/img/sc/dark/dashboard/2.png') },
    { title: 'Project', image: getImageUrl('/img/sc/dark/dashboard/3.png') },
    { title: 'Ecommerce', image: getImageUrl('/img/sc/dark/dashboard/4.png') }
  ],
  ecommerce: [
    { title: 'Product Listing', image: getImageUrl('/img/sc/dark/ecommerce/1.png') },
    { title: 'Product Details', image: getImageUrl('/img/sc/dark/ecommerce/2.png') },
    { title: 'Shopping Cart', image: getImageUrl('/img/sc/dark/ecommerce/7.png') },
    { title: 'Checkout Page', image: getImageUrl('/img/sc/dark/ecommerce/5.png') },
    { title: 'Customer List', image: getImageUrl('/img/sc/dark/ecommerce/6.png') },
    { title: 'Add New Product', image: getImageUrl('/img/sc/dark/ecommerce/3.png') },
    { title: 'Order List', image: getImageUrl('/img/sc/dark/ecommerce/4.png') },
  ],
  components: [
    { title: 'Accordion', image: getImageUrl('/img/sc/dark/components/1.png') },
    { title: 'Alert', image: getImageUrl('/img/sc/dark/components/2.png') },
    { title: 'Avatar', image: getImageUrl('/img/sc/dark/components/3.png') },
    { title: 'Button', image: getImageUrl('/img/sc/dark/components/4.png') },
    { title: 'Badge', image: getImageUrl('/img/sc/dark/components/5.png') },
    { title: 'Chip', image: getImageUrl('/img/sc/dark/components/6.png') },
    { title: 'Modal', image: getImageUrl('/img/sc/dark/components/7.png') },
    { title: 'List', image: getImageUrl('/img/sc/dark/components/8.png') },
    { title: 'Pagination', image: getImageUrl('/img/sc/dark/components/10.png') },
    { title: 'Progress', image: getImageUrl('/img/sc/dark/components/11.png') },
    { title: 'Tab', image: getImageUrl('/img/sc/dark/components/12.png') },
    { title: 'Tooltip', image: getImageUrl('/img/sc/dark/components/13.png') },
    { title: 'Dropdown', image: getImageUrl('/img/sc/dark/components/9.png') },
  ],
  apps: [
    { title: 'Kanban Board', image: getImageUrl('/img/sc/dark/apps/3.png') },
    { title: 'Chat', image: getImageUrl('/img/sc/dark/apps/5.png') },
    { title: 'Email', image: getImageUrl('/img/sc/dark/apps/4.png') },
    { title: 'Todo', image: getImageUrl('/img/sc/dark/apps/2.png') },
    { title: 'File Manager', image: getImageUrl('/img/sc/dark/apps/1.png') }
  ],
  auth: [
    { title: 'Login Page Basic', image: getImageUrl('/img/sc/dark/auth/1.png') },
    { title: 'Login Page Cover', image: getImageUrl('/img/sc/dark/auth/2.png') },
    { title: 'Signup Page Basic', image: getImageUrl('/img/sc/dark/auth/3.png') },
    { title: 'Signup Page Cover', image: getImageUrl('/img/sc/dark/auth/4.png') },
    { title: 'Verify Email', image: getImageUrl('/img/sc/dark/auth/5.png') },
    { title: 'Verify Email With Cover', image: getImageUrl('/img/sc/dark/auth/6.png') },
    { title: 'Reset Password', image: getImageUrl('/img/sc/dark/auth/7.png') },
    { title: 'Reset Password With Cover', image: getImageUrl('/img/sc/dark/auth/8.png') },
    { title: 'Forgot Password', image: getImageUrl('/img/sc/dark/auth/9.png') },
    { title: 'Forgot Password With Cover', image: getImageUrl('/img/sc/dark/auth/10.png') },
  ]
};

const filteredGalleryContent = computed(() => {
  if (!gallerySearchQuery.value) return galleryCategories[activeGalleryCategory.value];
  
  const query = gallerySearchQuery.value.toLowerCase();
  return galleryCategories[activeGalleryCategory.value].filter(item => 
    item.title.toLowerCase().includes(query)
  );
});

const displayedGalleryContent = computed(() => {
  if (gallerySearchQuery.value) return filteredGalleryContent.value;
  
  // Show all items instead of limiting
  return filteredGalleryContent.value;
});

const setGalleryCategory = (category) => {
  if (category === activeGalleryCategory.value) return;
  
  isGalleryLoading.value = true;
  gallerySearchQuery.value = '';
  displayLimit.value = initialItemsToShow;
  
  setTimeout(() => {
    activeGalleryCategory.value = category;
    isGalleryLoading.value = false;
  }, 300);
};

const loadMoreGallery = () => {
  displayLimit.value += initialItemsToShow;
};
</script>

<template>
  <div class="min-h-screen bg-white overflow-x-hidden">
    <!-- Header -->
    <header class="fixed top-0 left-0 right-0 bg-white shadow-premium z-50 py-5 w-full">
      <div class="container mx-auto px-6 flex justify-between items-center">
        <div class="text-2xl font-bold text-gray-900
        flex items-center gap-2
        font-heading tracking-wider uppercase">
          <Logo/>
          <span class="mt-2">
            JET<span class="text-coffee-primary">COFFEE</span>
          </span>
        </div>
        <nav class="hidden md:block">
          <ul class="flex gap-8">
            <li><a href="#features" class="text-gray-800 font-medium hover:text-coffee-primary transition-colors">Features</a></li>
            <li><a href="#light-mode" class="text-gray-800 font-medium hover:text-coffee-primary transition-colors">Light Mode</a></li>
            <li><a href="#dark-mode" class="text-gray-800 font-medium hover:text-coffee-primary transition-colors">Dark Mode</a></li>
            <li><a href="#tech-stack" class="text-gray-800 font-medium hover:text-coffee-primary transition-colors">Tech Stack</a></li>
            <li>
              <a href="#pricing" class="text-white font-medium
               bg-indigo-500 px-4 py-2 rounded-md
               hover:bg-indigo-600
               transform transition-all duration-300
               ring-2 ring-indigo-500">Buy Now</a>
            </li>
            <li><a href="#demo" class="font-medium
               text-indigo-500
               hover:ring-2 hover:text-indigo-800
               hover:bg-indigo-50
               transform transition-all duration-300
               ring-2 ring-indigo-500 px-4 py-2 rounded-md
              ">Live Preview</a></li>
          </ul>
        </nav>
      </div>
    </header>

    <!-- Hero Section -->
    <section class="py-32 bg-gray-900 text-white relative overflow-hidden">
      <!-- Decorative elements -->
      <div class="absolute top-0 left-0 w-full h-full overflow-hidden">
        <div class="absolute top-20 left-10 w-64 h-64 rounded-full bg-purple-600 opacity-5 blur-3xl"></div>
        <div class="absolute bottom-20 right-20 w-80 h-80 rounded-full bg-coffee-primary opacity-5 blur-3xl"></div>
        <div class="absolute top-1/3 right-1/4 w-40 h-40 rounded-full bg-blue-500 opacity-5 blur-3xl"></div>
      </div>
      
      <div class="container mx-auto px-6 flex flex-col md:flex-row items-center gap-16 relative z-10">
        <div class="flex-1 animate-slide-in-left">
          <h1 class="text-4xl md:text-6xl font-bold mb-6 font-heading leading-tight tracking-tight">
            A Complete <span class="bg-clip-text text-indigo-500">Admin UI Kit</span> built on 
            <span class="bg-clip-text  text-[#FF3E00]">SvelteKit</span>
          </h1>
          <p class="text-xl text-gray-300 mb-4 max-w-xl font-light leading-relaxed">
             I made this because I love <span class="text-[#FF3E00]">
              <img src="/img/logo/svelte.png" width="30" class="inline-block"/>
             </span> and <span class="text-indigo-500">Coffee</span>.
          </p>
          <div class="flex gap-6 flex-wrap">
            <div class="flex gap-4 items-center mt-4">
              <a href="#pricing" class="bg-indigo-500
              hover:bg-indigo-600
              text-white px-8 py-4 rounded font-medium shadow-premium
              transition-all hover:shadow-elegant hover:scale-102 flex items-center justify-center">
                <span>
                  Buy Now
                </span>
              </a>

              <a href="#demo" class="border-2 border-gray-700
              text-white px-8 py-4 rounded font-medium 
              transition-all hover:bg-gray-800 hover:border-coffee-primary">
                <span>
                  Live Preview
                </span>
              </a>
            </div>
          </div>
        </div>
        <div class="flex-1 animate-slide-in-right">
          <div class="relative transform perspective-1000  dashboard-preview-container  transition-all duration-700 top-[-160px]">
            <div class="absolute -inset-1 bg-gradient-to-r from-pink-500 via-purple-500 to-indigo-500 opacity-30 blur-md rounded-lg shadow-glow"></div>
            
            <img src="/img/sc/preview-2.png" alt="JETCOFFEE Dashboard Preview" class="
             w-full rounded-lg shadow-elegant hover:z-10 transform hover:scale-102 transition-transform duration-500
            absolute top-0 
            " />

            <img src="/img/sc/preview-3.png" alt="JETCOFFEE Dashboard Preview" class="
             w-full rounded-lg shadow-elegant hover:z-10 transform hover:scale-102 transition-transform duration-500
            absolute top-16 left-6
            "/>

            <img src="/img/sc/preview.png" alt="JETCOFFEE Dashboard Preview" class="
            absolute top-32 left-12 w-full rounded-lg hover:z-10 shadow-elegant transform hover:scale-102 transition-transform duration-500" />

           
          </div>
        </div>
      </div>
    </section>

    <!-- Features Section -->
    <section id="features" class="py-28 bg-white">
      <div class="container mx-auto px-6">
        <div class="text-center mb-20">
          <span class="text-coffee-primary font-medium uppercase tracking-wider text-sm font-heading">Premium Features</span>
          <h2 class="text-4xl md:text-5xl font-bold text-gray-900 mt-4 font-heading tracking-tight">Designed for Excellence</h2>
          <div class="w-24 h-1 bg-coffee-primary mx-auto mt-8 rounded-full"></div>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8 mb-20">
          <div class="bg-white rounded-lg p-10 shadow-premium border border-gray-100 transition-all hover:border-coffee-primary hover:-translate-y-2">
            <div class="w-16 h-16 bg-coffee-primary bg-opacity-10 rounded-lg flex items-center justify-center mb-6">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-coffee-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 21a4 4 0 01-4-4V5a2 2 0 012-2h4a2 2 0 012 2v12a4 4 0 01-4 4zm0 0h12a2 2 0 002-2v-4a2 2 0 00-2-2h-2.343M11 7.343l1.657-1.657a2 2 0 012.828 0l2.829 2.829a2 2 0 010 2.828l-8.486 8.485M7 17h.01" />
              </svg>
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-4 font-heading">Handcrafted Components</h3>
            <p class="text-gray-600">
              Handcrafted components that are designed to be used in your project.
            </p>
            <ul class="mt-6 space-y-2">
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                20+ Handcrafted UI Components 
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Easy to Customize
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Accessibility Focused
              </li>
            </ul>
          </div>

          <div class="bg-white rounded-lg p-10 shadow-premium border border-gray-100 transition-all hover:border-coffee-primary hover:-translate-y-2">
            <div class="w-16 h-16 bg-coffee-primary bg-opacity-10 rounded-lg flex items-center justify-center mb-6">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-coffee-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 5a1 1 0 011-1h14a1 1 0 011 1v2a1 1 0 01-1 1H5a1 1 0 01-1-1V5zM4 13a1 1 0 011-1h6a1 1 0 011 1v6a1 1 0 01-1 1H5a1 1 0 01-1-1v-6zM16 13a1 1 0 011-1h2a1 1 0 011 1v6a1 1 0 01-1 1h-2a1 1 0 01-1-1v-6z" />
              </svg>
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-4 font-heading">Rich Layout Components</h3>
            <p class="text-gray-600">Flexible layout systems that help you create complex interfaces with ease, from dashboards to data tables.</p>
            <ul class="mt-6 space-y-2">
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Grid Systems
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Advanced Panel Controls
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Nested Components
              </li>
            </ul>
          </div>

          <div class="bg-white rounded-lg p-10 shadow-premium border border-gray-100 transition-all hover:border-coffee-primary hover:-translate-y-2">
            <div class="w-16 h-16 bg-coffee-primary bg-opacity-10 rounded-lg flex items-center justify-center mb-6">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-coffee-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 18h.01M8 21h8a2 2 0 002-2V5a2 2 0 00-2-2H8a2 2 0 00-2 2v14a2 2 0 002 2z" />
              </svg>
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-4 font-heading">Responsive Design</h3>
            <p class="text-gray-600">Fully responsive layouts that adapt flawlessly to any device size, from desktops to mobile phones.</p>
            <ul class="mt-6 space-y-2">
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Mobile-First Approach
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Fluid Breakpoints
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Cross-Browser Compatible
              </li>
            </ul>
          </div>

          <div class="bg-white rounded-lg p-10 shadow-premium border border-gray-100 transition-all hover:border-coffee-primary hover:-translate-y-2">
            <div class="w-16 h-16 bg-coffee-primary bg-opacity-10 rounded-lg flex items-center justify-center mb-6">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-coffee-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" />
              </svg>
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-4 font-heading">6 Ready-to-Use Pre-Apps</h3>
            <p class="text-gray-600">Launch faster with our collection of pre-built applications ready for immediate implementation.</p>
            <ul class="mt-6 space-y-2">
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Email App
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Todo App
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                E-commerce Admin
              </li>
            </ul>
          </div>

          <div class="bg-white rounded-lg p-10 shadow-premium border border-gray-100 transition-all hover:border-coffee-primary hover:-translate-y-2">
            <div class="w-16 h-16 bg-coffee-primary bg-opacity-10 rounded-lg flex items-center justify-center mb-6">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-coffee-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" />
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
              </svg>
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-4 font-heading">Easy Customization</h3>
            <p class="text-gray-600">Tailor the dashboard to your exact needs with intuitive customization options and flexible settings.</p>
            <ul class="mt-6 space-y-2">
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                <p class="flex-1">
                  Easy to customize with Tailwind CSS
                </p>
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Pre-built components with variants
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
               Support Dark & Light Mode
              </li>
            </ul>
          </div>

          <div class="bg-white rounded-lg p-10 shadow-premium border border-gray-100 transition-all hover:border-coffee-primary hover:-translate-y-2">
            <div class="w-16 h-16 bg-coffee-primary bg-opacity-10 rounded-lg flex items-center justify-center mb-6">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-coffee-primary" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12h6m-6 4h6m2 5H7a2 2 0 01-2-2V5a2 2 0 012-2h5.586a1 1 0 01.707.293l5.414 5.414a1 1 0 01.293.707V19a2 2 0 01-2 2z" />
              </svg>
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-4 font-heading">Comprehensive Documentation</h3>
            <p class="text-gray-600">Get up and running quickly with our detailed documentation, tutorials, and code examples.</p>
            <ul class="mt-6 space-y-2">
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
                Step-by-Step Guides
              </li>
              <li class="flex items-center text-gray-700">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-coffee-primary mr-2" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
                </svg>
              <p class="flex-1">
                Detailed Documentation for each component
              </p>
              </li>
             
            </ul>
          </div>
        </div>

        <div class="bg-coffee-primary bg-opacity-5 rounded-2xl p-8 md:p-12">
          <div class="grid grid-cols-1 md:grid-cols-12 gap-8 items-center">
            <div class="md:col-span-7">
              <h3 class="text-2xl md:text-3xl font-bold text-gray-900 mb-4 font-heading">Ready-to-Use Applications</h3>
              <p class="text-gray-700 mb-6">Our 10 pre-built applications accelerate your development process, allowing you to launch your project in record time. Each application is fully functional and customizable.</p>
              
              <div class="grid grid-cols-2 md:grid-cols-2 gap-4">
                <div class="flex items-center space-x-2">
                  <div class="w-2 h-2 bg-coffee-primary rounded-full"></div>
                  <span>CRM Dashboard</span>
                </div>
                <div class="flex items-center space-x-2">
                  <div class="w-2 h-2 bg-coffee-primary rounded-full"></div>
                  <span>Analytics Portal</span>
                </div>
                <div class="flex items-center space-x-2">
                  <div class="w-2 h-2 bg-coffee-primary rounded-full"></div>
                  <span>E-commerce Admin</span>
                </div>
                <div class="flex items-center space-x-2">
                  <div class="w-2 h-2 bg-coffee-primary rounded-full"></div>
                  <span>Project Management</span>
                </div>
                <div class="flex items-center space-x-2">
                  <div class="w-2 h-2 bg-coffee-primary rounded-full"></div>
                  <span>Chat App</span>
                </div>
                <div class="flex items-center space-x-2">
                  <div class="w-2 h-2 bg-coffee-primary rounded-full"></div>
                  <span>Todo App</span>
                </div>
                <div class="flex items-center space-x-2">
                  <div class="w-2 h-2 bg-coffee-primary rounded-full"></div>
                  <span>Task Management</span>
                </div>
                <div class="flex items-center space-x-2">
                  <div class="w-2 h-2 bg-coffee-primary rounded-full"></div>
                  <span>Email App</span>
                </div>
                <div class="flex items-center space-x-2">
                  <div class="w-2 h-2 bg-coffee-primary rounded-full"></div>
                  <span>File Storage UI</span>
                </div>
              </div>
            </div>
            <div class="md:col-span-5">
              <div class="relative">
                <div class="absolute -inset-1 bg-gradient-gold opacity-20 blur-md rounded-lg"></div>
                <img src="./assets/dashboard-preview.svg" alt="Pre-applications Preview" class="relative w-full rounded-lg shadow-elegant" />
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Screenshots Section -->
    <section id="screenshots" class="py-28 bg-coffee-light">
      <div class="container mx-auto px-6">
        <div class="text-center mb-20">
          <span class="text-coffee-primary font-medium uppercase tracking-wider text-sm font-heading">Theme Variants</span>
          <h2 class="text-4xl md:text-2xl font-bold text-gray-900 mt-4 font-heading tracking-tight uppercase">Light Mode</h2>
          <div class="w-24 h-1 bg-coffee-primary mx-auto mt-2 rounded-full"></div>
        </div>
        
        <div class="flex flex-col lg:flex-row gap-10">
          <!-- Category Sidebar -->
          <div class="lg:w-1/4">
            <div class="bg-white rounded-xl shadow-premium p-6">
              <h3 class="text-xl font-bold mb-6 text-gray-900">Categories</h3>
              
              <ul class="space-y-3">
                <li v-for="(items, category) in categoryContent" :key="category">
                  <a href="#" 
                     @click.prevent="setActiveCategory(category)" 
                     :class="[
                       'flex items-center p-3 rounded-lg font-medium transition-all duration-300 relative overflow-hidden',
                       activeCategory === category 
                         ? 'bg-coffee-primary bg-opacity-10 text-coffee-primary shadow-sm' 
                         : 'hover:bg-coffee-primary hover:bg-opacity-5 text-gray-700'
                     ]">
                    <span class="absolute inset-y-0 left-0 w-1 bg-coffee-primary transform origin-left transition-all duration-300"
                          :class="activeCategory === category ? 'scale-y-100' : 'scale-y-0'"></span>
                    
                    <span class="relative z-10 flex items-center">
                      <!-- Category Icon -->
                      <svg xmlns="http://www.w3.org/2000/svg" 
                           class="h-5 w-5 mr-3 transition-transform duration-300"
                           :class="activeCategory === category ? 'rotate-0 scale-110' : 'rotate-0'"
                           fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path v-if="category === 'dashboard'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2V6z M14 6a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2V6z M4 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2H6a2 2 0 01-2-2v-2z M14 16a2 2 0 012-2h2a2 2 0 012 2v2a2 2 0 01-2 2h-2a2 2 0 01-2-2v-2z" />
                        <path v-else-if="category === 'ecommerce'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 11V7a4 4 0 00-8 0v4M5 9h14l1 12H4L5 9z" />
                        <path v-else-if="category === 'components'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" />
                        <path v-else-if="category === 'apps'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 3v2m6-2v2M9 19v2m6-2v2M5 9H3m2 6H3m18-6h-2m2 6h-2M7 19h10a2 2 0 002-2V7a2 2 0 00-2-2H7a2 2 0 00-2 2v10a2 2 0 002 2zM9 9h6v6H9V9z" />
                        <path v-else-if="category === 'auth'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
                      </svg>
                      
                      <!-- Category Name -->
                      <span class="capitalize">{{ category }}</span>
                    </span>
                    
                    <!-- Count badge -->
                    <span class="ml-auto bg-white bg-opacity-80 text-xs font-semibold px-2 py-1 rounded-full text-coffee-primary">
                      {{ items.length }}
                    </span>
                  </a>
                </li>
              </ul>
            </div>
          </div>
          
          <!-- Screenshot Preview Area -->
          <div class="lg:w-3/4">
            <!-- Title and search bar -->
            <div class="flex flex-col md:flex-row justify-between items-center mb-2">
              <h2 class="text-2xl font-bold text-gray-900 mb-4 md:mb-0 capitalize">{{ activeCategory }} Templates</h2>
              
              <!-- Search -->
              <div class="relative w-full md:w-auto">
                <input
                  v-model="searchQuery"
                  type="text"
                  placeholder="Search templates..."
                  class="w-full md:w-64 pl-10 pr-4 py-2 border border-gray-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-coffee-primary focus:border-transparent"
                />
                <div class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                  </svg>
                </div>
              </div>
            </div>
            
            <!-- Loading indicator -->
            <div v-if="isLoading" class="flex flex-col items-center justify-center py-20">
              <div class="w-12 h-12 border-4 border-coffee-primary border-t-transparent rounded-full animate-spin"></div>
              <p class="mt-4 text-coffee-primary font-medium">Loading {{ activeCategory }} screenshots...</p>
            </div>
            
            <!-- Content grid -->
            <div v-else>
              <!-- Results count -->
              <div v-if="filteredContent.length > 0" class="mb-6 text-sm text-gray-600">
                Showing {{ displayedContent.length }} of {{ filteredContent.length }} {{ activeCategory }} templates
              </div>
              
              <!-- Grid layout -->
              <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-2 gap-6">
                <!-- Dynamic Content Based on Active Category -->
                 <Card v-for="(item, index) in displayedContent" 
                     :key="index"
                     :item="item"
                     :activeCategory="activeCategory"
                     :index="index" />
              </div>
              
              <!-- Empty state -->
              <div v-if="filteredContent.length === 0" class="flex flex-col items-center justify-center py-16 bg-gray-50 rounded-lg">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
                </svg>
                <h3 class="mt-4 text-xl font-medium text-gray-700">No templates found</h3>
                <p class="mt-2 text-gray-500">Try a different search term or category</p>
                <button 
                  @click="searchQuery = ''" 
                  class="mt-6 px-4 py-2 bg-coffee-primary text-white rounded-md hover:bg-coffee-dark transition-colors"
                >
                  Clear Search
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    


    <!-- Gallery Section -->
    <section id="gallery" class="py-28 bg-gray-50">
      <div class="container mx-auto px-6">
        <div class="text-center mb-20">
          <span class="text-coffee-primary font-medium uppercase tracking-wider text-sm font-heading">Theme Variants</span>
          <h2 class="text-4xl md:text-2xl font-bold text-gray-900 mt-4 font-heading tracking-tight uppercase">Dark Mode</h2>
          <div class="w-24 h-1 bg-coffee-primary mx-auto mt-2 rounded-full"></div>
        </div>
        
        <div class="flex flex-col lg:flex-row gap-10">
          <!-- Category Sidebar -->
          <div class="lg:w-1/4">
            <div class="bg-white rounded-xl shadow-premium p-6">
              <h3 class="text-xl font-bold mb-6 text-gray-900">Categories</h3>
              
              <ul class="space-y-3">
                <li v-for="(items, category) in galleryCategories" :key="category">
                  <a href="#" 
                     @click.prevent="setGalleryCategory(category)" 
                     :class="[
                       'flex items-center p-3 rounded-lg font-medium transition-all duration-300 relative overflow-hidden',
                       activeGalleryCategory === category 
                         ? 'bg-coffee-primary bg-opacity-10 text-coffee-primary shadow-sm' 
                         : 'hover:bg-coffee-primary hover:bg-opacity-5 text-gray-700'
                     ]">
                    <span class="absolute inset-y-0 left-0 w-1 bg-coffee-primary transform origin-left transition-all duration-300"
                          :class="activeGalleryCategory === category ? 'scale-y-100' : 'scale-y-0'"></span>
                    
                    <span class="relative z-10 flex items-center">
                      <!-- Category Icon -->
                      <svg xmlns="http://www.w3.org/2000/svg" 
                           class="h-5 w-5 mr-3 transition-transform duration-300"
                           :class="activeGalleryCategory === category ? 'rotate-0 scale-110' : 'rotate-0'"
                           fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path v-if="category === 'dashboard'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 01-9 9m9-9a9 9 0 00-9-9m9 9H3m9 9a9 9 0 01-9-9m9 9c1.657 0 3-4.03 3-9s-1.343-9-3-9m0 18c-1.657 0-3-4.03-3-9s1.343-9 3-9m-9 9a9 9 0 019-9" />
                        <path v-else-if="category === 'ecommerce'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 18h.01M8 21h8a2 2 0 002-2V5a2 2 0 00-2-2H8a2 2 0 00-2 2v14a2 2 0 002 2z" />
                        <path v-else-if="category === 'components'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.75 17L9 20l-1 1h8l-1-1-.75-3M3 13h18M5 17h14a2 2 0 002-2V5a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                        <path v-else-if="category === 'apps'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
                        <path v-else-if="category === 'auth'" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
                      </svg>
                      
                      <!-- Category Name -->
                      <span class="capitalize">{{ category }}</span>
                    </span>
                    
                    <!-- Count badge -->
                    <span class="ml-auto bg-white bg-opacity-80 text-xs font-semibold px-2 py-1 rounded-full text-coffee-primary">
                      {{ items.length }}
                    </span>
                  </a>
                </li>
              </ul>
            </div>
          </div>
          
          <!-- Gallery Preview Area -->
          <div class="lg:w-3/4">
            <!-- Title and search bar -->
            <div class="flex flex-col md:flex-row justify-between items-center mb-2">
              <h2 class="text-2xl font-bold text-gray-900 mb-4 md:mb-0 capitalize">{{ activeGalleryCategory }} Projects</h2>
              
              <!-- Search -->
              <div class="relative w-full md:w-auto">
                <input
                  v-model="gallerySearchQuery"
                  type="text"
                  placeholder="Search projects..."
                  class="w-full md:w-64 pl-10 pr-4 py-2 border border-gray-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-coffee-primary focus:border-transparent"
                />
                <div class="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
                  </svg>
                </div>
              </div>
            </div>
            
            <!-- Loading indicator -->
            <div v-if="isGalleryLoading" class="flex flex-col items-center justify-center py-20">
              <div class="w-12 h-12 border-4 border-coffee-primary border-t-transparent rounded-full animate-spin"></div>
              <p class="mt-4 text-coffee-primary font-medium">Loading {{ activeGalleryCategory }} projects...</p>
            </div>
            
            <!-- Content grid -->
            <div v-else>
              <!-- Results count -->
              <div v-if="filteredGalleryContent.length > 0" class="mb-6 text-sm text-gray-600">
                Showing {{ displayedGalleryContent.length }} of {{ filteredGalleryContent.length }} {{ activeGalleryCategory }} projects
              </div>
              
              <!-- Grid layout -->
              <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-2 gap-6">
                <!-- Dynamic Content Based on Active Category -->
             

                <Card v-for="(item, index) in displayedGalleryContent" 
                     :key="index"
                     :item="item"
                     :activeGalleryCategory="activeGalleryCategory"
                     :index="index" />
              </div>
              
              <!-- Empty state -->
              <div v-if="filteredGalleryContent.length === 0" class="flex flex-col items-center justify-center py-16 bg-gray-50 rounded-lg">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16 text-gray-300" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
                </svg>
                <h3 class="mt-4 text-xl font-medium text-gray-700">No projects found</h3>
                <p class="mt-2 text-gray-500">Try a different search term or category</p>
                <button 
                  @click="gallerySearchQuery = ''" 
                  class="mt-6 px-4 py-2 bg-coffee-primary text-white rounded-md hover:bg-coffee-dark transition-colors"
                >
                  Clear Search
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>


    <!-- Testimonials Section -->
    <section id="testimonials" class="mb-12 bg-white">
      <div class="container mx-auto px-6">
        <div class="text-center mb-20">
          <span class="text-coffee-primary font-medium uppercase tracking-wider text-sm font-heading">Technologies</span>
          <h2 class="text-4xl md:text-2xl font-bold text-gray-900 mt-4 font-heading tracking-tight uppercase">Built with Modern Stack</h2>
          <div class="w-24 h-1 bg-coffee-primary mx-auto mt-2 rounded-full"></div>
        </div>
        
        <div class="grid grid-cols-2 md:grid-cols-3 gap-10">
          <div class="bg-white p-8 rounded-xl shadow-premium border border-gray-100 text-center hover:border-coffee-primary hover:-translate-y-2 transition-all">
            <div class="w-20 h-20 mx-auto mb-4 flex items-center justify-center">
              <svg viewBox="0 0 24 24" class="w-16 h-16" xmlns="http://www.w3.org/2000/svg"><path fill="#F7DF1E" d="M0 0h24v24H0V0zm22.034 18.276c-.175-1.095-.888-2.015-3.003-2.873-.736-.345-1.554-.585-1.797-1.14-.091-.33-.105-.51-.046-.705.15-.646.915-.84 1.515-.66.39.12.75.42.976.9 1.034-.676 1.034-.676 1.755-1.125-.27-.42-.404-.601-.586-.78-.63-.705-1.469-1.065-2.834-1.034l-.705.089c-.676.165-1.32.525-1.71 1.005-1.14 1.291-.811 3.541.569 4.471 1.365 1.02 3.361 1.244 3.616 2.205.24 1.17-.87 1.545-1.966 1.41-.811-.18-1.26-.586-1.755-1.336l-1.83 1.051c.21.48.45.689.81 1.109 1.74 1.756 6.09 1.666 6.871-1.004.029-.09.24-.705.074-1.65l.046.067zm-8.983-7.245h-2.248c0 1.938-.009 3.864-.009 5.805 0 1.232.063 2.363-.138 2.711-.33.689-1.18.601-1.566.48-.396-.196-.597-.466-.83-.855-.063-.105-.11-.196-.127-.196l-1.825 1.125c.305.63.75 1.172 1.324 1.517.855.51 2.004.675 3.207.405.783-.226 1.458-.691 1.811-1.411.51-.93.402-2.07.397-3.346.012-2.054 0-4.109 0-6.179l.004-.056z"/><path fill="#F7DF1E" d="M0 0h24v24H0V0zm22.034 18.276c-.175-1.095-.888-2.015-3.003-2.873-.736-.345-1.554-.585-1.797-1.14-.091-.33-.105-.51-.046-.705.15-.646.915-.84 1.515-.66.39.12.75.42.976.9 1.034-.676 1.034-.676 1.755-1.125-.27-.42-.404-.601-.586-.78-.63-.705-1.469-1.065-2.834-1.034l-.705.089c-.676.165-1.32.525-1.71 1.005-1.14 1.291-.811 3.541.569 4.471 1.365 1.02 3.361 1.244 3.616 2.205.24 1.17-.87 1.545-1.966 1.41-.811-.18-1.26-.586-1.755-1.336l-1.83 1.051c.21.48.45.689.81 1.109 1.74 1.756 6.09 1.666 6.871-1.004.029-.09.24-.705.074-1.65l.046.067zm-8.983-7.245h-2.248c0 1.938-.009 3.864-.009 5.805 0 1.232.063 2.363-.138 2.711-.33.689-1.18.601-1.566.48-.396-.196-.597-.466-.83-.855-.063-.105-.11-.196-.127-.196l-1.825 1.125c.305.63.75 1.172 1.324 1.517.855.51 2.004.675 3.207.405.783-.226 1.458-.691 1.811-1.411.51-.93.402-2.07.397-3.346.012-2.054 0-4.109 0-6.179l.004-.056z"/></svg>
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-2">JavaScript</h3>
            <p class="text-gray-600">Core language for dynamic web applications and interactivity.</p>
          </div>
          
          <div class="bg-white p-8 rounded-xl shadow-premium border border-gray-100 text-center hover:border-coffee-primary hover:-translate-y-2 transition-all">
            <div class="w-20 h-20 mx-auto mb-4 flex items-center justify-center">
              <img src="/img/logo/ts.png" alt="TypeScript" class="w-16 h-16">
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-2">TypeScript</h3>
            <p class="text-gray-600">Strongly typed programming language that builds on JavaScript.</p>
          </div>
          
          <div class="bg-white p-8 rounded-xl shadow-premium border border-gray-100 text-center hover:border-coffee-primary hover:-translate-y-2 transition-all">
            <div class="w-20 h-20 mx-auto mb-4 flex items-center justify-center">
              <img src="/img/logo/tailwindcss.png" alt="TailwindCSS" class="w-16 h-16">
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-2">TailwindCSS</h3>
            <p class="text-gray-600">Utility-first CSS framework for rapidly building custom designs.</p>
          </div>
          
          <div class="bg-white p-8 rounded-xl shadow-premium border border-gray-100 text-center hover:border-coffee-primary hover:-translate-y-2 transition-all">
            <div class="w-20 h-20 mx-auto mb-4 flex items-center justify-center">
              <img src="/img/logo/svelte.png" alt="Svelte" class="w-16 h-16">
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-2">Svelte</h3>
            <p class="text-gray-600">Radical new approach to building user interfaces with minimal boilerplate.</p>
          </div>
          
          <div class="bg-white p-8 rounded-xl shadow-premium border border-gray-100 text-center hover:border-coffee-primary hover:-translate-y-2 transition-all">
            <div class="w-20 h-20 mx-auto mb-4 flex items-center justify-center">
              <img src="/img/logo/vite.png" alt="Vite" class="w-16 h-16">
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-2">Vite</h3>
            <p class="text-gray-600">Next generation frontend tooling for blazing fast development experience.</p>
          </div>
          
          <div class="bg-white p-8 rounded-xl shadow-premium border border-gray-100 text-center hover:border-coffee-primary hover:-translate-y-2 transition-all">
            <div class="w-20 h-20 mx-auto mb-4 flex items-center justify-center">
              <img src="/img/logo/postcss.png" alt="PostCSS" class="w-16 h-16">
            </div>
            <h3 class="text-2xl font-bold text-gray-900 mb-2">PostCSS</h3>
            <p class="text-gray-600">Tool for transforming CSS with JavaScript to enhance workflow and capabilities.</p>
          </div>
        </div>
      </div>
    </section>

    <a href="#pricing"
       class="bg-coffee-primary
       md:w-1/4 w-full mx-auto
       mb-8
       text-md
       uppercase
       font-semibold
       text-white px-4 py-4 rounded-lg shadow-premium
       transition-all duration-300 hover:bg-indigo-600 hover:scale-105 flex items-center justify-center gap-2">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4M7 13L5.4 5M7 13l-2.293 2.293c-.63.63-.184 1.707.707 1.707H17m0 0a2 2 0 100 4 2 2 0 000-4zm-8 2a2 2 0 11-4 0 2 2 0 014 0z" />
      </svg>
      <span>
        Buy Now
      </span>
    </a>


    <!-- Footer -->
    <footer class="bg-coffee-charcoal text-gray-400 py-12">
      <div class="container mx-auto px-6">
        <div class="flex justify-center mb-6">
          <div class="text-2xl font-bold text-white font-heading tracking-wider uppercase">JETCOFFEE</div>
        </div>
        
      
        
        <div class="flex justify-center gap-6 mb-8">
          <a href="#" class="hover:text-white transition-colors">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24"><path d="M24 4.557c-.883.392-1.832.656-2.828.775 1.017-.609 1.798-1.574 2.165-2.724-.951.564-2.005.974-3.127 1.195-.897-.957-2.178-1.555-3.594-1.555-3.179 0-5.515 2.966-4.797 6.045-4.091-.205-7.719-2.165-10.148-5.144-1.29 2.213-.669 5.108 1.523 6.574-.806-.026-1.566-.247-2.229-.616-.054 2.281 1.581 4.415 3.949 4.89-.693.188-1.452.232-2.224.084.626 1.956 2.444 3.379 4.6 3.419-2.07 1.623-4.678 2.348-7.29 2.04 2.179 1.397 4.768 2.212 7.548 2.212 9.142 0 14.307-7.721 13.995-14.646.962-.695 1.797-1.562 2.457-2.549z"></path></svg>
          </a>
          <a href="#" class="hover:text-white transition-colors">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.237 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"></path></svg>
          </a>
          <a href="#" class="hover:text-white transition-colors">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24"><path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"></path></svg>
          </a>
        </div>
        
        <div class="text-center border-t border-gray-800 pt-6">
          <p>© {{ new Date().getFullYear() }} JETCOFFEE. All rights reserved.</p>
        </div>
      </div>
    </footer>
  </div>
</template>

<style>
/* Custom animations and effects */
@keyframes slide-in-left {
  from {
    transform: translateX(-50px);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

@keyframes slide-in-right {
  from {
    transform: translateX(50px);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

.animate-slide-in-left {
  animation: slide-in-left 1s ease-out;
}

.animate-slide-in-right {
  animation: slide-in-right 1s ease-out;
}

.animate-fade-in {
  animation: fade-in 0.5s ease-out forwards;
  opacity: 0;
}

@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* 3D Transforms */
.perspective-1000 {
  perspective: 1000px;
}

.rotate-y-3 {
  transform: rotateY(3deg);
}

.rotate-x-5 {
  transform: rotateX(5deg);
}

.hover\:rotate-y-5:hover {
  transform: rotateY(5deg);
}

.hover\:rotate-x-8:hover {
  transform: rotateX(8deg);
}

/* Gradient effects */
.bg-gradient-luxury {
  background: linear-gradient(to right, var(--coffee-primary), var(--coffee-secondary));
}

.bg-gradient-gold {
  background: linear-gradient(to right, #f7971e, #ffd200);
}

.shadow-premium {
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}

.shadow-elegant {
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.12);
}

.shadow-glow {
  box-shadow: 0 0 40px rgba(236, 72, 153, 0.4);
}

.scale-102 {
  transform: scale(1.02);
}

.hover\:scale-102:hover {
  transform: scale(1.02);
}

/* Hero image glow effect */
.dashboard-preview-container {
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.dashboard-preview-container::before {
  content: '';
  position: absolute;
  top: -10px;
  left: -10px;
  right: -10px;
  bottom: -10px;
  background: linear-gradient(45deg, rgba(236, 72, 153, 0.3), rgba(124, 58, 237, 0.3), rgba(99, 102, 241, 0.3));
  border-radius: 12px;
  filter: blur(20px);
  z-index: -1;
}
</style>
