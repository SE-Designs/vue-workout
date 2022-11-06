<template>
  <Navigation />
  <router-view />
  <div v-if="appReady" class="min-h-full font-Poppins box-border">
    <h1>App is ready</h1>
  </div>
  <div v-else>
    <h2>Not Ready</h2>
  </div>
</template>

<script>
import { ref } from "vue";
import store from "./store/index";
import { supabase } from "./supabase/init";
import Navigation from "./components/Navigation.vue";
export default {
  components: {
    Navigation,
  },
  setup() {
    // Create variables:
    const appReady = ref(null);

    // Check if the user is already logged in:
    const user = supabase.auth.getSession();

    // If the user does not exist, make app ready:
    if (!user) {
      appReady.value = true;
    }

    // On auth state change:
    supabase.auth.onAuthStateChange((_, session) => {
      store.methods.setUser(session);
      appReady.value = true;
    });

    return { appReady };
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700&display=swap");
</style>
