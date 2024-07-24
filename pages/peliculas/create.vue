<template>
  <div class="flex items-center justify-center p-12">
    <div class="mx-auto w-full max-w-[550px]">
      <form @submit.prevent="registerPelicula" @keydown="createForm.onKeydown($event)" >
        <div class="mb-5">
          <label for="title" class="mb-3 block text-base font-medium text-[#07074D]">
           Nombre
          </label>
          <input
            type="text"
            v-model="createForm.title" name="title" :class="{ 'is-invalid': createForm.errors.has('title') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base font-medium text-[#6B7280] outline-none focus:border-[#6A64F1] focus:shadow-md"
          />
          <has-error :form="createForm" field="title"></has-error>
        </div>
        <div class="mb-5">
          <label for="publish_date" class="mb-3 block text-base font-medium text-[#07074D]">
           F. de publicación
          </label>
          <input
            type="date"
            v-model="createForm.publish_date" name="publish_date" :class="{ 'is-invalid': createForm.errors.has('publish_date') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base font-medium text-[#6B7280] outline-none focus:border-[#6A64F1] focus:shadow-md"
          />
          <has-error :form="createForm" field="publish_date"></has-error>
        </div>
        <div class="mb-5">
           <image-input v-model="createForm.image_url" />
        </div>
        <div class="mb-5">
          <label for="status" class="mb-3 block text-base font-medium text-[#07074D]">
            Activo?
          </label>
          <input
            type="checkbox"
            v-model="createForm.status" name="status" placeholder="Enter status" :class="{ 'is-invalid': createForm.errors.has('status') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base w-[1.5rem] h-[1.5rem] font-medium text-[#6B7280] outline-none"
          />
          <has-error :form="createForm" field="status"></has-error>
        </div>
        <div>
          <button type="submit" :disabled="createForm.busy" 
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
import ImageInput from '/components/ImageInput.vue';
export default {
  middleware: "auth",
  data() {
    
    return {
      createForm: this.$vform({
        title: "",
        publish_date: "",
        image_url: "",
        status:0,
      }),
    };
  },
  methods: {
  async registerPelicula() {
    try {
      const formData = new FormData();
      formData.append('title', this.createForm.title);
      formData.append('publish_date', this.createForm.publish_date);
      formData.append('image_url', this.createForm.image_url);
      formData.append('status', this.createForm.status);

      const response = await this.$axios.post("/auth/peliculas", formData, {
        headers: {
          'Content-Type': 'multipart/form-data',
        },
      });

      this.$toast.success({
        title: "Success!",
        message: "Registration Successful",
      });
      this.createForm.reset();
      this.$router.push('/peliculas');
    } catch (err) {
      console.log(err);
    }
  },
}
};
</script>

<style></style>
