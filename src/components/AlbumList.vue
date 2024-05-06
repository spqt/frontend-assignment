<template>
  <div>
    <div
      class="flex flex-col-reverse lg:flex-row justify-between items-center mt-3"
    >
      <div>
        <h1 class="text-3xl md:text-4xl font-thin mt-3 ml-5 mb-5 uppercase">
          Albums of the <span class="font-bold">Month</span>.
        </h1>
      </div>
      <div
        class="flex flex-row justify-end items-center sm:mt-10 lg:mt-0 mb-4 mr-5"
      >
        <div class="w-23 mx-4 my-2">
          <SearchBar v-model="searchQuery" />
        </div>
        <label for="genreFilter" class="font-thin ml-2 sm:ml-0 mr-2"
          >Filter by genre:</label
        >
        <select id="genreFilter" v-model="selectedGenre">
          <option value="">All Genres</option>
          <option
            v-for="genre in [...new Set(albums.map((album) => album.genre))]"
            :value="genre"
          >
            {{ genre }}
          </option>
        </select>
      </div>
    </div>
    <div class="w-full flex flex-row gap-x-15 justify-between bg-white">
      <button
        @click="changePage(currentPage - 1)"
        :disabled="currentPage === 1"
        class="font-light text-5xl mr-6 rotate-180 hover:text-[#abb8c3] bg-black text-white p-6 invisible lg:visible rounded-l-md transition duration-300 transform"
      >
        ➤
      </button>
      <ul
        :class="[
          'grid grid-cols-1 lg:grid-cols-3 gap-[25px] max-w-sm mx-auto md:max-w-none md:mx-0',
        ]"
      >
        <li
          v-for="album in paginatedAlbums"
          :key="album.id"
          class="font-thin shadow-md bg-black p-2 md:p-7 text-white rounded-md hover:bg-opacity-30 group transition duration-300 transform cursor-pointer"
          @click="toggleGenreVisibility(album.id)"
        >
          <img
            :src="album.image"
            alt=""
            class="max-h-[400px] sm:max-h-[600px] rounded-lg group-hover:scale-105 transition duration-300 transform"
          />
          <div class="flex flex-col items-start">
            <div class="font-normal mt-3">
              {{ album.name }}
            </div>
            <div>{{ album.artist }}</div>
            <div class="text-sm mb-10">{{ album.year }}</div>
            <transition name="fade" appear>
              <div
                v-if="genreVisible[album.id]"
                class="text-md md:text-lg mt-2"
              >
                Genre: {{ album.genre }}
              </div>
            </transition>
          </div>
        </li>
      </ul>
      <button
        @click="changePage(currentPage + 1)"
        :disabled="currentPage === totalPages"
        class="font-light text-5xl ml-6 hover:text-[#abb8c3] bg-black text-white p-5 invisible lg:visible rounded-l-md transition duration-300 transform"
      >
        ➤
      </button>
    </div>
    <div class="flex flex-col">
      <div class="flex flex-row justify-between py-10 text-sm">
        <button
          @click="changePage(currentPage - 1)"
          :disabled="currentPage === 1"
          class="font-light text-3xl ml-5 hover:text-[#abb8c3] bg-white text-black visible lg:invisible"
        >
          ←
        </button>
        <span class="font-thin pt-3"
          >page {{ currentPage }} of {{ totalPages }}</span
        >
        <button
          @click="changePage(currentPage + 1)"
          :disabled="currentPage === totalPages"
          class="font-light text-3xl mr-5 hover:text-[#abb8c3] bg-white text-black visible lg:invisible"
        >
          →
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import albumsData from "../data/albums.json";
import SearchBar from "./SearchBar.vue";

export default {
  components: {
    SearchBar,
  },
  data() {
    return {
      albums: albumsData.albums,
      currentPage: 1,
      itemsPerPage: 3,
      searchQuery: "",
      selectedGenre: "",
      genreVisible: {},
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.filteredAlbums.length / this.itemsPerPage);
    },
    filteredAlbums() {
      let filtered = this.albums;

      if (this.selectedGenre) {
        filtered = filtered.filter(
          (album) => album.genre === this.selectedGenre
        );
      }

      filtered.sort((a, b) => b.year - a.year);

      if (this.searchQuery) {
        filtered = filtered.filter((album) =>
          album.name.toLowerCase().includes(this.searchQuery.toLowerCase())
        );
      }

      return filtered;
    },
    paginatedAlbums() {
      const start = (this.currentPage - 1) * this.itemsPerPage;
      const end = start + this.itemsPerPage;
      return this.filteredAlbums.slice(start, end);
    },
  },
  methods: {
    changePage(page) {
      if (page >= 1 && page <= this.totalPages) {
        this.currentPage = page;
      }
    },
    toggleGenreVisibility(albumId) {
      if (this.genreVisible[albumId]) {
        this.genreVisible[albumId] = false;
      } else {
        for (let id in this.genreVisible) {
          this.genreVisible[id] = false;
        }
        this.genreVisible[albumId] = true;
      }
    },
  },
};
</script>

<style scoped>
.fade-enter-active,
.fade-appear-active {
  transition: opacity 0.5s ease-in;
}

.fade-enter-from,
.fade-appear {
  opacity: 0;
}
</style>
