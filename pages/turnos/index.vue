<template>
  <div class="sm:mx-0.5 lg:mx-0.5">
    <div class="flex flex-col md:flex-row justify-between">
      <h1>Turnos</h1>
      <nuxt-link :to="{ name: 'turnos-create' }"
        class="flex items-center justify-center h-12 w-48 overflow-hidden rounded-2xl bg-green-500 text-md text-white"
      >
        Crear Turno
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
                    Turno
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
                    Disponibilidad
                  </th>
                  <th
                    scope="col"
                    class="text-sm font-medium text-gray-900 px-6 py-4 text-left"
                  ></th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(turno, index) in turnos"
                  :key="index"
                  class="bg-white border-b transition duration-300 ease-in-out hover:bg-gray-100"
                >
                  <td
                    class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900"
                  >
                    {{ turno.id }}
                  </td>
                  <td
                    class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                  >
                    {{ turno.turno }}
                  </td>
                  <td
                    class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap"
                  >
                    <span
                      v-if="turno.status == 1"
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
                      v-if="turno.available == 1"
                      class="inline-flex items-center gap-1 rounded-full bg-green-50 px-2 py-1 text-xs font-semibold text-green-600"
                    >
                      Disponible
                    </span>
                    <span
                      v-else
                      class="inline-flex items-center gap-1 rounded-full bg-red-50 px-2 py-1 text-xs font-semibold text-red-600"
                    >
                      No Disponible
                    </span>
                  </td>

                  <td
                    class="text-sm text-gray-900 font-light px-6 py-4 whitespace-nowrap px-3 py-2"
                  >
                    <div class="flex justify-end gap-4">
                      <nuxt-link :to="`/turnos/${turno.id}`" class="cursor-pointer">
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
                      <button @click="deleteTurno(turno.id)" class="text-red-600 hover:text-red-900 focus:outline-none">
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
    </div>
  </div>
</template>

<script>
export default {
  middleware: "auth",
  data() {
    return {
      turnos: [], // Definir el arreglo 'turnos' en el estado del componente
    };
  },
  mounted() {
    this.getTurnos(); // Llamar al método 'getTurnos' cuando el componente se monte
  },
  methods: {
    async getTurnos() {
      try {
        let response = await this.$axios.get("/auth/turnos"); // Hacer la solicitud a la API para obtener los turnos
        this.turnos = response.data.turnos; // Asignar los datos obtenidos a la variable 'turnos'
        //console.log(this.turnos);
      } catch (err) {
        console.error(err); // Mostrar el error en la consola
        this.$toast.error({
          title: "Error!",
          message: "Failed to load turnos",
        });
      }
    },

    async deleteTurno(id) {
      try {
        let response = await this.$axios.post("auth/turnos/" + id + "/delete"); // Hacer la solicitud a la API para obtener los turnos
        this.$toast.success({
          title: "Success!",
          message: "Deleted",
        });

        this.getTurnos()

      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>
