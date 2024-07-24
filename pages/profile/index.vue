<template>
  <div class="flex items-center justify-center p-12">
    <div class="mx-auto w-full max-w-[550px]">
      <form @submit.prevent="updateProfile" @keydown="userForm.onKeydown($event)" >
        <div class="mb-5">
          <label for="name" class="mb-3 block text-base font-medium text-[#07074D]">
            Nombre
          </label>
          <input
            type="text"
            v-model="userForm.name" name="name" placeholder="Enter name" :class="{ 'is-invalid': userForm.errors.has('name') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base font-medium text-[#6B7280] outline-none focus:border-[#6A64F1] focus:shadow-md"
          />
          <has-error :form="userForm" field="name"></has-error>
        </div>
        <div class="mb-5">
          <label for="email" class="mb-3 block text-base font-medium text-[#07074D]">
            Email Address
          </label>
          <input
            type="text"
            v-model="userForm.email" name="email" placeholder="Enter email" :class="{ 'is-invalid': userForm.errors.has('email') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base font-medium text-[#6B7280] outline-none focus:border-[#6A64F1] focus:shadow-md"
          />
          <has-error :form="userForm" field="email"></has-error>
        </div>
        <div class="mb-5">
          <label for="password" class="mb-3 block text-base font-medium text-[#07074D]">
            Password
          </label>
          <input
            type="password" 
            v-model="userForm.password" name="password" placeholder="Enter password" :class="{ 'is-invalid': userForm.errors.has('password') }"
            class="w-full rounded-md border border-[#e0e0e0] bg-white py-3 px-6 text-base font-medium text-[#6B7280] outline-none focus:border-[#6A64F1] focus:shadow-md"
          />
          <has-error :form="userForm" field="password"></has-error>
        </div>
        <div>
          <button type="submit" :disabled="userForm.busy" 
            class="hover:shadow-form rounded-md bg-[#6A64F1] py-3 px-8 text-base font-semibold text-white outline-none"
          >
            Actualizar
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
      userForm: this.$vform({
        name: "",
        email: "",
        password: "",
      }),
    };
  },
  methods: {
    async updateProfile() {
      try {
        let { data } = await this.userForm.post("/auth/profile");
        this.userForm.password = "";
        await this.$auth.fetchUser();

        this.$toast.success({
          title: "Success!",
          message: "Profile updated successfully",
        });
      } catch (err) {
        console.log(err);
      }
    },
    getUser() {
      let user = this.$store.state.auth.user;

      this.userForm.name = user.name;
      this.userForm.email = user.email;
    },
  },
  mounted() {
    this.getUser();
  },
};
</script>

<style></style>
