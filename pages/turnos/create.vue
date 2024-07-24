<template>
  <div class="flex items-center justify-center p-12">
    <div class="mx-auto w-full max-w-[550px]">
      <form @submit.prevent="registerTurno" @keydown="createForm.onKeydown($event)">
        <div class="mb-5">
          <label for="turno" class="mb-3 block text-base font-medium text-[#07074D]">
            Turno
          </label>
          <input
            type="time"
            v-model="createForm.turno"
            name="turno"
            :class="{ 'is-invalid': createForm.errors.has('turno') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base font-medium text-[#6B7280] outline-none focus:border-[#6A64F1] focus:shadow-md"
          />
          <has-error :form="createForm" field="turno"></has-error>
        </div>
        <div class="mb-5">
          <label for="status" class="mb-3 block text-base font-medium text-[#07074D]">
            Activo?
          </label>
          <input
            type="checkbox"
            v-model="createForm.status"
            name="status"
            placeholder="Enter status"
            :class="{ 'is-invalid': createForm.errors.has('status') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base w-[1.5rem] h-[1.5rem] font-medium text-[#6B7280] outline-none"
          />
          <has-error :form="createForm" field="status"></has-error>
        </div>
        <div>
          <button
            type="submit"
            :disabled="createForm.busy"
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
export default {
  middleware: "auth",
  data() {
    return {
      createForm: this.$vform({
        turno: "",
        status: false,
      }),
    };
  },
  methods: {
    async registerTurno() {
      try {
        let data = await this.createForm.post("/auth/turnos");

        this.$toast.success({
          title: "Success!",
          message: "Registration Successful",
        });

        this.createForm.reset();
        this.$router.push("/turnos");
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style></style>
