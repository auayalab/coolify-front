<template>
  <!-- component -->
  <div class="flex items-center justify-center h-screen bg-gray-200">
    <div class="container">
      <div class="bg-white rounded-lg shadow-lg p-5 md:p-20 mx-2">
        <div class="text-center">
          <h2
            class="text-4xl tracking-tight leading-10 font-extrabold text-gray-900 sm:text-5xl sm:leading-none md:text-6xl"
          >
            <span v-if="loggedIn" class="text-indigo-600">Bienvenido, {{ user.name }}</span>
          </h2>
        </div>
   
      </div>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    loggedIn() {
      return this.$store.state.auth.loggedIn;
    },
    user() {
      return this.$store.state.auth.user;
    },
  },
};
</script>

<style></style>
