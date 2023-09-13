

<template>
  <header class="w-full text-center p-4">
    <h1 class="font-bela text-blue-500 text-4xl md:text-5xl">
        <span>Image</span>
        <span class="text-black">Search</span>
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-10 h-10 inline">
            <path d="M11.625 16.5a1.875 1.875 0 100-3.75 1.875 1.875 0 000 3.75z" />
            <path fill-rule="evenodd" d="M5.625 1.5H9a3.75 3.75 0 013.75 3.75v1.875c0 1.036.84 1.875 1.875 1.875H16.5a3.75 3.75 0 013.75 3.75v7.875c0 1.035-.84 1.875-1.875 1.875H5.625a1.875 1.875 0 01-1.875-1.875V3.375c0-1.036.84-1.875 1.875-1.875zm6 16.5c.66 0 1.277-.19 1.797-.518l1.048 1.048a.75.75 0 001.06-1.06l-1.047-1.048A3.375 3.375 0 1011.625 18z" clip-rule="evenodd" />
            <path d="M14.25 5.25a5.23 5.23 0 00-1.279-3.434 9.768 9.768 0 016.963 6.963A5.23 5.23 0 0016.5 7.5h-1.875a.375.375 0 01-.375-.375V5.25z" />
        </svg>
    </h1>
  </header>
  <main>
    <section>
      <form @submit.prevent="search" class="p-4 md:p-8">   
        <label for="default-search" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white">Search</label>
        <div class="relative">
            <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                <svg class="w-4 h-4 text-gray-500 dark:text-gray-400" aria-hidden="true" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 20 20">
                    <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"/>
                </svg>
            </div>
            <input @input="watchedSearch" v-model="searchTerm" type="search" id="default-search" class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" placeholder="Enter your product name here..." required>
            <button type="submit" class="text-white absolute right-2.5 bottom-2.5 bg-blue-500 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-500 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Search</button>
        </div>
    </form>
    </section>
    <section>
      <div v-if="!showGallery" class="text-center p-4 mt-4 md:p-8">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" class="w-16 h-16 mx-auto">
              <path fill-rule="evenodd" d="M1.5 6a2.25 2.25 0 012.25-2.25h16.5A2.25 2.25 0 0122.5 6v12a2.25 2.25 0 01-2.25 2.25H3.75A2.25 2.25 0 011.5 18V6zM3 16.06V18c0 .414.336.75.75.75h16.5A.75.75 0 0021 18v-1.94l-2.69-2.689a1.5 1.5 0 00-2.12 0l-.88.879.97.97a.75.75 0 11-1.06 1.06l-5.16-5.159a1.5 1.5 0 00-2.12 0L3 16.061zm10.125-7.81a1.125 1.125 0 112.25 0 1.125 1.125 0 01-2.25 0z" clip-rule="evenodd" />
          </svg>
          <p>Please enter your product name in the search bar.</p>
      </div>
      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 p-4 mb-4 md:p-8 md:mb-8">
          <div v-for="image in images" :key="image.id" class="relative">
              <div class="h-0 pb-[100%]">
                  <img class="absolute inset-0 w-full h-full object-contain rounded-lg" :src="image.src" alt="">
              </div>
          </div>
      </div>
    </section>
  </main>
</template>


<script setup>

  import { computed, ref } from 'vue';
  import axios from 'axios';

  const searchTerm = ref("");

  const images = ref([]);

  const showGallery = computed(()=> {
    if(images.value.length === 0 || searchTerm === ''){
      return false;
    }else{
      return true;
    }
  });

  const search = () => {
    axios.get(`https://api.pexels.com/v1/search?query=${searchTerm.value}&per_page=20`, {
        headers: {
            Authorization: 'h9SBDFcdfChz0kjtSgWiXi5NXznUalaA8ju9l6lMp8IBN58tZLOQFHVH'
        }
    }).then(response => {
        images.value = response.data.photos.map(photo =>({
            id: photo.id,
            src: photo.src.medium
        }))
        console.log(images.value);
    }).catch(error => {
        console.error('Erreur lors de la récupération des données :', error);
    });
  }

  const watchedSearch = () => {
      setTimeout(search, 1000); 
  }

</script>
