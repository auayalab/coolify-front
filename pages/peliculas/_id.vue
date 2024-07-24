<template>
  <div class="flex items-center justify-center p-12">
    <div class="mx-auto w-full max-w-[550px]">
      <form @submit.prevent="updatePelicula" @keydown="updateForm.onKeydown($event)">
        <div class="mb-5">
          <label for="title" class="mb-3 block text-base font-medium text-[#07074D]">
            Nombre
          </label>
          <input
            type="text"
            v-model="updateForm.title"
            name="title"
            :class="{ 'is-invalid': updateForm.errors.has('title') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base font-medium text-[#6B7280] outline-none focus:border-[#6A64F1] focus:shadow-md"
          />
          <has-error :form="updateForm" field="title"></has-error>
        </div>
        <div class="mb-5">
          <label for="publish_date" class="mb-3 block text-base font-medium text-[#07074D]">
            F. de publicación
          </label>
          <input
            type="date"
            v-model="updateForm.publish_date"
            name="publish_date"
            :class="{ 'is-invalid': updateForm.errors.has('publish_date') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base font-medium text-[#6B7280] outline-none focus:border-[#6A64F1] focus:shadow-md"
          />
          <has-error :form="updateForm" field="publish_date"></has-error>
        </div>
        <div class="mb-5 flex">
          <img
            :src="this.baseURL + this.pelicula.image_url"
            class="w-[200px] h-[200px]"
          />
          <image-input-update class="ml-3" v-model="updateForm.image_url" />
        </div>
        <div class="mb-5">
          <label for="status" class="mb-3 block text-base font-medium text-[#07074D]">
            Activo?
          </label>
          <input
            type="checkbox"
            v-model="updateForm.status"
            name="status"
            placeholder="Enter status"
            :class="{ 'is-invalid': updateForm.errors.has('status') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base w-[1.5rem] h-[1.5rem] font-medium text-[#6B7280] outline-none"
          />
          <has-error :form="updateForm" field="status"></has-error>
        </div>
        <div>
          <button
            type="submit"
            :disabled="updateForm.busy"
            class="hover:shadow-form rounded-md bg-[#6A64F1] py-3 px-8 text-base font-semibold text-white outline-none"
          >
            Guardar
          </button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import ImageInputUpdate from "/components/ImageInputUpdate.vue";
export default {
  data() {
    return {
      peliculaId: "",
      pelicula: [],
      baseURL: "",
      updateForm: this.$vform({
        id: "", // Add the id field
        title: "",
        publish_date: "",
        image_url: "",
        status: "",
      }),
    };
  },
  methods: {
    async updatePelicula() {
      try {
        const formData = new FormData();
        formData.append('id', this.updateForm.id);
        formData.append('title', this.updateForm.title);
        formData.append('publish_date', this.updateForm.publish_date);
        if (this.updateForm.image_url instanceof File) {
          formData.append('image_url', this.updateForm.image_url);
        }
        formData.append('status', this.updateForm.status ? 1 : 0);

        let { data } = await this.$axios.post("/auth/peliculas/update", formData, {
          headers: {
            'Content-Type': 'multipart/form-data',
          },
        });

        this.$toast.success({
          title: "Success!",
          message: "Profile updated successfully",
        });
        this.$router.push("/peliculas");
      } catch (err) {
        console.log(err);
      }
    },
    async getPelicula() {
      try {
        let response = await this.$axios.get(
          "/auth/peliculas/" + this.peliculaId + "/show"
        );
        this.pelicula = response.data.pelicula;
        this.updateForm.title = this.pelicula.title;
        this.updateForm.publish_date = this.pelicula.publish_date;
        this.updateForm.image_url = this.pelicula.image_url;
        this.updateForm.status = this.pelicula.status;
        this.updateForm.id = this.$route.params.id;
        this.baseURL = this.$config.baseURL;
      } catch (err) {
        console.error(err);
        this.$toast.error({
          title: "Error!",
          message: "Failed to load pelicula",
        });
      }
    },
  },
  mounted() {
    this.peliculaId = this.$route.params.id;
    this.getPelicula();
  },
};
</script>
