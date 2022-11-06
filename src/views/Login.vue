<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- Error Handling -->
    <div v-if="errorMsg" class="mb-10 p-4 rounded-md bg-light-grey shadow-lg">
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- Login Form -->
    <form
      @submit.prevent="login"
      class="p-8 flex flex-col bg-light-grey rounded-md shadow-lg"
    >
      <h2 class="text-3xl text-at-light-green mb-4 font-semibold text-center">
        Login
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

      <button
        type="submit"
        class="mt-6 py-2 px-6 rounded-md self-center text-base font-medium text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
      >
        Login
      </button>

      <span class="text-sm mt-6 text-center"
        >Don't have an account?
        <router-link
          class="text-at-light-green cursor-pointer underline"
          :to="{ name: 'Register' }"
          >Register</router-link
        ></span
      >
    </form>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";
import { useRouter } from "vue-router";
export default {
  name: "register",
  setup() {
    // Create variables:
    const router = useRouter();

    const email = ref(null);
    const password = ref(null);
    const errorMsg = ref(null);

    // Login function:
    const login = async () => {
      try {
        const { error } = await supabase.auth.signInWithPassword({
          email: email.value,
          password: password.value,
        });

        if (error) throw error;
        router.push({ name: "Home" });
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = null;
        }, 5000);
      }
    };

    return { email, password, errorMsg, login };
  },
};
</script>

<style></style>
