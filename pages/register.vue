<template>
  <div class="font-sans">
    <div
      class="relative min-h-screen flex flex-col sm:justify-center items-center bg-gray-100"
    >
      <div class="relative sm:max-w-sm w-full">
        <div
          class="card bg-blue-400 shadow-lg w-full h-full rounded-3xl absolute transform -rotate-6"
        ></div>
        <div
          class="card bg-red-400 shadow-lg w-full h-full rounded-3xl absolute transform rotate-6"
        ></div>
        <div class="relative w-full rounded-3xl px-6 py-4 bg-gray-100 shadow-md">
          <label
            for=""
            class="block mt-3 text-sm text-gray-700 text-center font-semibold"
          >
            Registro
          </label>
          <form
            class="mt-10"
            @submit.prevent="registerUser"
            @keydown="registerForm.onKeydown($event)"
          >
            <div>
              <input
                type="text"
                v-model="registerForm.name"
                placeholder="Nombre"
                name="name"
                class="mt-1 p-2 block w-full border-none bg-gray-100 h-11 rounded-xl shadow-lg hover:bg-blue-100 focus:bg-blue-100 focus:ring-0"
                :class="{ 'is-invalid': registerForm.errors.has('name') }"
              />
              <has-error :form="registerForm" field="name"></has-error>
            </div>
            <div>
              <input
                type="text"
                v-model="registerForm.email"
                placeholder="Correo electronico"
                name="email"
                class="mt-1 p-2 block w-full border-none bg-gray-100 h-11 rounded-xl shadow-lg hover:bg-blue-100 focus:bg-blue-100 focus:ring-0"
                :class="{ 'is-invalid': registerForm.errors.has('email') }"
              />
              <has-error :form="registerForm" field="email"></has-error>
            </div>

            <div class="mt-7">
              <input
                type="password"
                v-model="registerForm.password"
                placeholder="Contraseña"
                name="passwrd"
                class="mt-1 block p-2 w-full border-none bg-gray-100 h-11 rounded-xl shadow-lg hover:bg-blue-100 focus:bg-blue-100 focus:ring-0"
                :class="{ 'is-invalid': registerForm.errors.has('password') }"
              />
              <has-error :form="registerForm" field="password"></has-error>
            </div>
            <div class="mt-7">
              <button
                :disabled="registerForm.busy"
                type="submit"
                class="bg-blue-500 w-full py-3 rounded-xl text-white shadow-xl hover:shadow-inner focus:outline-none transition duration-500 ease-in-out transform hover:-translate-x hover:scale-105"
              >
                Registrarse
              </button>
            </div>

            <div class="mt-7">
              <div class="flex justify-center items-center">
                <label class="mr-2">¿Ya tienes cuenta?</label>
                <nuxt-link
                  :to="{ name: 'login' }"
                  href="#"
                  class="text-blue-500 transition duration-500 ease-in-out transform hover:-translate-x hover:scale-105"
                >
                  Inicia Sesión
                </nuxt-link>
              </div>
            </div>
          </form>
          <div class="alert alert-danger" v-if="registerForm.errors.errors.account">
            {{ registerForm.errors.errors.account[0] }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  auth: "guest",
  data() {
    return {
      registerForm: this.$vform({
        name: "",
        email: "",
        password: "",
      }),
    };
  },
  methods: {
    async registerUser() {
      try {
        let data = await this.registerForm.post("/auth/register");

        await this.$auth.setUserToken(data.access_token);

        this.$toast.success({
          title: "Success!",
          message: "Registration Successful",
        });

        this.registerForm.reset();
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style></style>
