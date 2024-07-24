<template>
  <div v-if="!loggedIn" class="font-sans">
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
            Login
          </label>
          <form
            class="mt-10"
            @submit.prevent="userLogin"
            @keydown="loginForm.onKeydown($event)"
          >
            <div>
              <input
                type="text"
                v-model="loginForm.email"
                placeholder="Correo electronico"
                name="email"
                class="mt-1 p-2 block w-full border-none bg-gray-100 h-11 rounded-xl shadow-lg hover:bg-blue-100 focus:bg-blue-100 focus:ring-0"
                :class="{ 'is-invalid': loginForm.errors.has('email') }"
              />
              <has-error :form="loginForm" field="email"></has-error>
            </div>

            <div class="mt-7">
              <input
                type="password"
                v-model="loginForm.password"
                placeholder="Contraseña"
                name="passwrd"
                class="mt-1 block p-2 w-full border-none bg-gray-100 h-11 rounded-xl shadow-lg hover:bg-blue-100 focus:bg-blue-100 focus:ring-0"
                :class="{ 'is-invalid': loginForm.errors.has('password') }"
              />
              <has-error :form="loginForm" field="password"></has-error>
            </div>

            <div class="mt-7 flex">
              <label
                for="remember_me"
                class="inline-flex items-center w-full cursor-pointer"
              >
                <input
                  id="remember_me"
                  type="checkbox"
                  class="rounded border-gray-300 text-indigo-600 shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
                  name="remember"
                />
                <span class="ml-2 text-sm text-gray-600"> Recuerdame </span>
              </label>

              <div class="w-full text-right">
                <nuxt-link
                  :to="{ name: 'forgot' }"
                  class="underline text-sm text-gray-600 hover:text-gray-900"
                >
                  ¿Olvidó su contraseña?
                </nuxt-link>
              </div>
            </div>
            <div class="mt-7">
              <div class="flex justify-center items-center">
                <label class="mr-2">¿Eres nuevo?</label>
                <nuxt-link
                  :to="{ name: 'register' }"
                  class="text-blue-500 transition duration-500 ease-in-out transform hover:-translate-x hover:scale-105"
                >
                  Crea una cuenta
                </nuxt-link>
              </div>
            </div>

            <div class="mt-7">
              <button
                :disabled="loginForm.busy"
                type="submit"
                class="bg-blue-500 w-full py-3 rounded-xl text-white shadow-xl hover:shadow-inner focus:outline-none transition duration-500 ease-in-out transform hover:-translate-x hover:scale-105"
              >
                Login
              </button>
            </div>
          </form>
          <div class="alert alert-danger" v-if="loginForm.errors.errors.account">
            {{ loginForm.errors.errors.account[0] }}
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
      loginForm: this.$vform({
        email: "admin@example.com",
        password: "password",
      }),
    };
  },
  methods: {
    async userLogin() {
      try {
        let { data } = await this.loginForm.post("/auth/login");
        await this.$auth.setUserToken(data.access_token);

        this.$toast.success({
          title: "Success!",
          message: "Welcome",
        });

        this.loginForm.reset();
      } catch (err) {
        console.log(err);
      }
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
