<template>
  <div v-if="loggedIn" class="flex flex-col h-screen bg-gray-100">
    <!-- Barra de navegación superior -->
    <div class="bg-white text-white shadow w-full p-2 flex items-center justify-between">
      <div class="flex items-center">
        <div class="hidden md:flex items-center">

          <h2 class="font-bold p-6 mr-2 text-black text-xl">Evaluación OSP</h2>
        </div>
        <div class="md:hidden flex items-center">
          <!-- Se muestra solo en dispositivos pequeños -->
          <button id="menuBtn">
            <i class="fas fa-bars text-gray-500 text-lg"></i>
            <!-- Ícono de menú -->
          </button>
        </div>
      </div>
    </div>

    <!-- Contenido principal -->
    <div class="flex-1 flex">
      <!-- Barra lateral de navegación (oculta en dispositivos pequeños) -->
      <div class="p-2 bg-white w-60 flex flex-col hidden md:flex" id="sideNav">
        <nav>
          <nuxt-link :to="{ name: 'dashboard' }"
            class="block text-gray-500 py-2.5 px-4 my-4 rounded transition duration-200 hover:bg-gradient-to-r hover:from-cyan-400 hover:to-cyan-300 hover:text-white"
          >
            <i class="fas fa-home mr-2"></i>Inicio
          </nuxt-link>
          <nuxt-link :to="{ name: 'peliculas' }"
            class="block text-gray-500 py-2.5 px-4 my-4 rounded transition duration-200 hover:bg-gradient-to-r hover:from-cyan-400 hover:to-cyan-300 hover:text-white"
          >
            <i class="fas fa-home mr-2"></i>Peliculas
          </nuxt-link>

           <nuxt-link :to="{ name: 'turnos' }"
            class="block text-gray-500 py-2.5 px-4 my-4 rounded transition duration-200 hover:bg-gradient-to-r hover:from-cyan-400 hover:to-cyan-300 hover:text-white"
          >
            <i class="fas fa-home mr-2"></i>Turnos
          </nuxt-link>

          <nuxt-link :to="{ name: 'profile' }"
            class="block text-gray-500 py-2.5 px-4 my-4 rounded transition duration-200 hover:bg-gradient-to-r hover:from-cyan-400 hover:to-cyan-300 hover:text-white"
          >
            <i class="fas fa-home mr-2"></i>Perfil
          </nuxt-link>

        </nav>

        <!-- Ítem de Cerrar Sesión -->
        <a
          class="block text-gray-500 py-2.5 px-4 my-2 rounded transition duration-200 hover:bg-gradient-to-r hover:from-cyan-400 hover:to-cyan-300 hover:text-white mt-auto"
          @click.prevent="logout"
        >
          <i class="fas fa-sign-out-alt mr-2"></i>Cerrar sesión
        </a>

        <!-- Señalador de ubicación -->
        <div class="bg-gradient-to-r from-cyan-300 to-cyan-500 h-px mt-2"></div>
      </div>

      <!-- Área de contenido principal -->
        <div class="flex-1 p-4">
             <Nuxt />
        </div>
      </div>
    </div>
  </div>
  <div v-else-if="!loggedIn">
    <Nuxt />
  </div>
</template>

<script>
export default {
  middleware: "auth",
  methods: {
    async logout() {
      await this.$auth.logout();
    },
  },
  computed: {
    loggedIn() {
      return this.$store.state.auth.loggedIn;
    },
  },
};
</script>

<style></style>
