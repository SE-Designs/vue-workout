<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- Error Handling -->
    <div v-if="errorMsg" class="mb-10 p-4 rounded-md bg-light-grey shadow-lg">
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- Register Form -->
    <form
      @submit.prevent="register"
      class="p-8 flex flex-col bg-light-grey rounded-md shadow-lg"
    >
      <h2 class="text-3xl text-at-light-green mb-4 font-semibold text-center">
        Register
      </h2>
      <div class="flex flex-col mb-3">
        <label for="email" class="mb-1 text-lg text-at-light-green"
          >Email:</label
        >
        <input
          type="text"
          id="email"
          class="p-2 text-gray-500 focus:outline-none rounded-md"
          required
          v-model="email"
        />
      </div>
      <div class="flex flex-col mb-3">
        <label for="password" class="mb-1 text-lg text-at-light-green"
          >Password:</label
        >
        <input
          type="password"
          id="password"
          class="p-2 text-gray-500 focus:outline-none rounded-md"
          required
          v-model="password"
        />
      </div>
      <div class="flex flex-col mb-3">
        <label for="confirmPassword" class="mb-1 text-lg text-at-light-green"
          >Confirm Password:</label
        >
        <input
          type="password"
          id="confirmPassword"
          class="p-2 text-gray-500 focus:outline-none rounded-md"
          required
          v-model="confirmPassword"
        />
      </div>

      <button
        type="submit"
        class="mt-6 py-2 px-6 rounded-md self-center text-base font-medium text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
      >
        Register
      </button>

      <span class="text-sm mt-6 text-center"
        >Already have an account?
        <router-link
          class="text-at-light-green cursor-pointer underline"
          :to="{ name: 'Login' }"
          >Login</router-link
        ></span
      >
    </form>
  </div>
</template>

<script>
import { ref } from "vue";
import { getSupabase } from "../supabase/init";
import { useRouter } from "vue-router";

export default {
  name: "register",
  setup() {
    // Create variables:
    const router = useRouter();

    const email = ref(null);
    const password = ref(null);
    const confirmPassword = ref(null);
    const errorMsg = ref(null);

    // Register Function:
    const register = async () => {
      if (password.value === confirmPassword.value) {
        try {
          const { error } = await getSupabase().auth.signUp({
            email: email.value,
            password: password.value,
          });

          if (error) throw error;
          router.push({ name: "Login" });
        } catch (error) {
          errorMsg.value = error.message;
          setTimeout(() => {
            errorMsg.value = null;
          }, 5000);
        }

        return;
      }

      errorMsg.value = "Error: Passwords does not match!";
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    };
    return { email, password, confirmPassword, errorMsg, register };
  },
};
</script>

<style></style>
