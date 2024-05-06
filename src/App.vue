<template>
  <div class="h-screen w-full flex bg-white flex-col">
    <Navbar />
    <div class="mx-auto my-auto text-[#333]">
      <div className="flex text-center justify-center mt-10">
        <AlbumsList />
      </div>
    </div>
    <Footer />
  </div>
</template>

<script setup>
import AlbumsList from "./components/AlbumList.vue";
import Navbar from "./components/Navbar.vue";
import Footer from "./components/Footer.vue";
</script>

<style></style>
