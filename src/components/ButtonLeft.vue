<template>
  <button
    @click="changePage(currentPage - 1)"
    :disabled="currentPage === 1"
    class="font-light text-5xl mr-10 rotate-180 hover:text-[#abb8c3] bg-black text-white p-5 invisible lg:visible rounded-l-md"
  >
    ➤
  </button>
</template>

<script>
export default {
  name: "ButtonLeft",
};
</script>
