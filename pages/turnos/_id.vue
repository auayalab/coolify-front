<template>
  <div class="flex items-center justify-center p-12">
    <div class="mx-auto w-full max-w-[550px]">
      <form @submit.prevent="updateTurno" @keydown="updateForm.onKeydown($event)">
        <div class="mb-5">
          <label for="turno" class="mb-3 block text-lg mb-3 font-medium text-[#07074D]">
            Editar Turno
          </label>
          <input id="turnoId" name="turnoId" type="hidden" v-model="updateForm.id"   />
          <input
            type="time"
            id="turno"
            name="turno"
            v-model="updateForm.turno"  
            :class="{ 'is-invalid': updateForm.errors.has('turno') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base font-medium text-[#6B7280] outline-none focus:border-[#6A64F1] focus:shadow-md"
          />
          <has-error :form="updateForm" field="turno"></has-error>
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
export default {
  data() {
    return {
      turnoId: "",
      turno: [],
      updateForm: this.$vform({
        id: "",
        turno: "",
        status: "",
      }),
    };
  },
  methods: {
    async updateTurno() {
      try {
        let { data } = await this.updateForm.post("/auth/turnos/update");
        this.$toast.success({
          title: "Success!",
          message: "Profile updated successfully",
        });
        this.$router.push('/turnos');
      } catch (err) {
        console.log(err);
      }
    },
    async getTurno() {
      try {
        let response = await this.$axios.get("/auth/turnos/" + this.turnoId + "/show");
        this.turno = response.data.turno;
        this.updateForm.turno = this.turno.turno;
        this.updateForm.status = this.turno.status;
        this.updateForm.id = this.$route.params.id;
        
      } catch (err) {
        console.error(err);
        this.$toast.error({
          title: "Error!",
          message: "Failed to load turnos",
        });
      }
    },
  },
  mounted() {
    this.turnoId = this.$route.params.id;
    this.getTurno();
  },
};
</script>

<style></style>
