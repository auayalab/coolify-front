<template>
  <div class="sm:mx-0.5 lg:mx-0.5">
    <div class="flex flex-col md:flex-row justify-between">
      <h1>Peliculas</h1>
      <nuxt-link
        :to="{ name: 'peliculas-create' }"
        class="flex items-center justify-center h-12 w-48 overflow-hidden rounded-2xl bg-green-500 text-md text-white"
      >
        Crear Pelicula
        <div
          class="absolute inset-0 h-full w-full scale-0 rounded-2xl transition-all duration-300 group-hover:scale-100 group-hover:bg-white/30"
        ></div>
      </nuxt-link>
    </div>

    <!-- component -->
    <div class="flex flex-col">
      <div class="overflow-x-auto">
        <div class="py-2 inline-block min-w-full">
          <div class="overflow-hidden">
            <table class="min-w-full">
              <thead class="bg-gray-200 border-b">
                <tr>
                  <th
                    scope="col"
                    class="text-sm font-medium text-gray-900 px-6 py-4 text-left"
                  >
                    #
                  </th>
                  <th
                    scope="col"
                    class="text-sm font-medium text-gray-900 px-6 py-4 text-left"
                  >
                    Nombre
                  </th>
                  <th
                    scope="col"
                    class="text-sm font-medium text-gray-900 px-6 py-4 text-left"
                  >
                    Estado
                  </th>
                  <th
                    scope="col"
                    class="text-sm font-medium text-gray-900 px-6 py-4 text-left"
                  >
                    Turno
                  </th>

                  <th
                    scope="col"
                    class="text-sm font-medium text-gray-900 px-6 py-4 text-left"
                  ></th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(pelicula, index) in peliculas"
                  :key="index"
                  class="bg-white border-b transition duration-300 ease-in-out hover:bg-gray-100"
                >
                  <td
                    class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
                  >
                    {{ pelicula.id }}
                  </td>
                  <td
                    class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                  >
                    {{ pelicula.title }}
                  </td>
                  <td
                    class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                  >
                    <span
                      v-if="pelicula.status == 1"
                      class="inline-flex items-center gap-1 rounded-full bg-green-50 px-2 py-1 text-xs font-semibold text-green-600"
                    >
                      Activo
                    </span>
                    <span
                      v-else
                      class="inline-flex items-center gap-1 rounded-full bg-red-50 px-2 py-1 text-xs font-semibold text-red-600"
                    >
                      Inactivo
                    </span>
                  </td>
                  <td
                    class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                  >
                    <span
                      v-if="pelicula.turno != null"
                      class="inline-flex items-center gap-1 rounded-full bg-green-50 px-2 py-1 text-xs font-semibold text-green-600"
                    >
                      {{ pelicula.turno.turno }}
                    </span>
                    <span
                      v-else
                      class="inline-flex items-center gap-1 rounded-full bg-red-50 px-2 py-1 text-xs font-semibold text-red-600"
                    >
                      Sin Asignar
                    </span>
                  </td>

                  <td
                    class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap px-3 py-2"
                  >
                    <div class="flex justify-end gap-4">
                      <button
                        id="assing"
                        @click="openAssignModal(pelicula.id)"
                        class="text-red-600 hover:text-red-900 focus:outline-none"
                      >
                        <!-- Icono de asignar turno -->
                        <svg
                          class="h-6 w-6"
                          viewBox="0 0 12 12"
                          enable-background="new 0 0 12 12"
                          version="1.1"
                          xml:space="preserve"
                          xmlns="http://www.w3.org/2000/svg"
                          xmlns:xlink="http://www.w3.org/1999/xlink"
                        >
                          <g>
                            <rect fill="#1D1D1B" height="1" width="11" x="0.5" y="5.5" />
                            <rect fill="#1D1D1B" height="1" width="11" x="0.5" y="2.5" />
                            <rect fill="#1D1D1B" height="1" width="11" x="0.5" y="8.5" />
                          </g>
                        </svg>
                      </button>
                      <nuxt-link :to="`/peliculas/${pelicula.id}`" class="cursor-pointer">
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          fill="none"
                          viewBox="0 0 24 24"
                          stroke="currentColor"
                          class="h-6 w-6"
                        >
                          <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M16.862 4.487l1.687-1.688a1.875 1.875 0 112.652 2.652L6.832 19.82a4.5 4.5 0 01-1.897 1.13l-2.685.8.8-2.685a4.5 4.5 0 011.13-1.897L16.863 4.487zm0 0L19.5 7.125"
                          />
                        </svg>
                      </nuxt-link>
                      <button
                        @click="deletePelicula(pelicula.id)"
                        class="text-red-600 hover:text-red-900 focus:outline-none"
                      >
                        <svg
                          xmlns="http://www.w3.org/2000/svg"
                          fill="none"
                          viewBox="0 0 24 24"
                          stroke="currentColor"
                          class="h-6 w-6"
                        >
                          <path
                            stroke-linecap="round"
                            stroke-linejoin="round"
                            d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0"
                          />
                        </svg>
                      </button>
                    </div>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <div
        v-if="showModal == true"
        class="fixed inset-0 flex items-center justify-center"
      >
        <div class="fixed inset-0 bg-black opacity-50"></div>
        <div class="relative bg-white p-8 rounded-lg shadow-xl max-w-md">
          <h3 class="text-lg font-semibold mb-4">Asignar Turno</h3>
          <!-- Mostrar el ID de la película -->
          <p class="text-gray-500 mb-2">ID de la película: {{ movieId }}</p>
          <div class="mb-4">
            <label for="turno">Selecciona un turno:</label>
            <select
              id="turno"
              v-model="selectedTurno"
              class="block w-full mt-1 py-2 px-3 border border-gray-300 bg-white rounded-md shadow-sm focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"
            >
              <option :value="null" disabled>Selecciona un turno</option>
              <option v-for="turno in turnos" :key="turno.id" :value="turno.id">
                {{ turno.turno }}
              </option>
            </select>
          </div>
          <div class="mt-6 flex justify-end">
            <button @click="showModal = false" class="px-4 py-2 text-gray-600 mr-2">
              Cancelar
            </button>
            <button @click="asignarTurno()" class="px-4 py-2 bg-red-500 text-white">
              Asignar
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  middleware: "auth",
  data() {
    return {
      turnos: [], // Definir el arreglo 'turnos' en el estado del componente
      peliculas: [],
      showModal: false,
      movieId: null,
      selectedTurno: null,
    };
  },
  mounted() {
    this.getData(); // Llamar al método 'getTurnos' cuando el componente se monte
  },
  methods: {
    async getData() {
      try {
        let response = await this.$axios.get("/auth/peliculas"); // Hacer la solicitud a la API para obtener los turnos
        this.turnos = response.data.turnos; // Asignar los datos obtenidos a la variable 'turnos'
        this.peliculas = response.data.peliculas;
      } catch (err) {
        console.error(err); // Mostrar el error en la consola
        this.$toast.error({
          title: "Error!",
          message: "Failed to load data",
        });
      }
    },
    async deletePelicula(id) {
      try {
        let response = await this.$axios.post("auth/peliculas/" + id + "/delete");
        this.$toast.success({
          title: "Success!",
          message: "Deleted",
        });

        this.getData();
      } catch (err) {
        console.log(err);
      }
    },
    openAssignModal(id) {
      this.movieId = id;
      this.showModal = true;
    },
    async asignarTurno() {
      try {
        let response = await this.$axios.post(`auth/peliculas/${this.movieId}/assign`, {
          turnoId: this.selectedTurno,
        });
        this.$toast.success({
          title: "Success!",
          message: "Turno asignado correctamente",
        });
        this.showModal = false;
        this.getData();
      } catch (err) {
        console.error(err);
        this.$toast.error({
          title: "Error!",
          message: "No se pudo asignar el turno",
        });
      }
    },
  },
};
</script>
