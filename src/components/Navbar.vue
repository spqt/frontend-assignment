<script>
import SearchBar from "./SearchBar.vue";

export default {
  name: "Navbar",
  components: {
    SearchBar,
  },
};
</script>

<template>
  <nav class="top-0 left-0 w-screen bg-black bg-opacity-90">
    <div
      class="max-w-screen-xl flex flex-wrap items-center justify-between mx-auto py-8"
    >
      <a href="/" class="flex items-center space-x-3 ml-10 sm:ml-0">
        <img
          src="https://brightbid.com/wp-content/uploads/2023/06/logo.svg"
          class="h-15"
          alt=""
        />
      </a>
      <div className="flex flex-row gap-x-10 invisible sm:visible">
        <a
          href="/"
          class="text-white text-lg font-thin hover:text-gray-300 transition duration-200 ease-in-out"
          >ABOUT</a
        >
        <a
          href="/"
          class="text-white text-lg font-thin hover:text-gray-300 transition duration-200 ease-in-out"
          >CONTACT</a
        >
      </div>
    </div>
  </nav>
</template>
