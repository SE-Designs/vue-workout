<template>
  <header class="bg-at-light-green text-white">
    <nav
      class="container py-5 px-4 flex flex-col gap-4 items-center sm:flex-row"
    >
      <router-link
        class="flex items-center gap-x-4 cursor-pointer"
        :to="{ name: 'Home' }"
      >
        <img class="w-14" src="../assets/images/dumbbell-light.png" alt="" />
        <h1 class="text-lg font-semibold">Workout Tracker</h1>
      </router-link>
      <ul class="flex flex-1 justify-end gap-x-10">
        <router-link class="cursor-pointer" :to="{ name: 'Home' }"
          >Home</router-link
        >
        <router-link v-if="user" class="cursor-pointer" :to="{ name: 'Create' }"
          >Create</router-link
        >
        <router-link v-if="!user" class="cursor-pointer" :to="{ name: 'Login' }"
          >Login</router-link
        >
        <li v-if="user" @click="logout" class="cursor-pointer">Logout</li>
      </ul>
    </nav>
  </header>
</template>

<script>
import store from "../store";
import { computed } from "vue";
import { useRouter } from "vue-router";
import { getSupabase } from "../supabase/init";
export default {
  setup() {
    // Get the user from store:
    const user = computed(() => store.state.user);

    // Setup ref to the router:
    const router = useRouter();

    // Logout function:
    const logout = async () => {
      await getSupabase().auth.signOut();
      router.push({ name: "Home" });
    };

    return { logout, user };
  },
};
</script>

<style scoped></style>
