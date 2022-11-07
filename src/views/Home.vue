<template>
  <div v-if="dataLoaded" class="container mt-10 px-4">
    <!-- EMPTY -->
    <div v-if="data.length === 0" class="w-full flex flex-col items-center">
      <h2 class="text-2xl">Nothing to show here!</h2>
      <p>Do not lazy, create first!</p>
      <router-link
        class="mt-6 py-2 px-6 rounded-md self-center text-base font-medium text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
        :to="{ name: 'Create' }"
        >Create Workout</router-link
      >
    </div>
    <!-- DATA -->
    <div
      v-else
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
    >
      <router-link
        class="flex flex-col items-center bg-light-grey p-8 shadow-md cursor-pointer hover:opacity-70 transition-all"
        :to="{ name: 'View-Workout', params: { workoutId: workout.id } }"
        v-for="(workout, index) of data"
        :key="index"
      >
        <!-- Cardio Image -->
        <img
          v-if="workout.workoutType === 'cardio'"
          src="../assets/images/running-light-green.png"
          class="h-24 w-auto"
        />
        <!-- Strength Image -->
        <img
          v-if="workout.workoutType === 'strength'"
          src="../assets/images/dumbbell-light-green.png"
          class="h-24 w-auto"
        />
        <h3
          class="mt-8 mb-2 text-center text-xl font-extrabold text-at-light-green"
        >
          {{ workout.workoutName }}
        </h3>
        <span
          class="mt-6 py-1 px-2 text-xs uppercase font-bold text-white bg-at-light-green shadow-md rounded-lg"
          >{{ workout.workoutType }}</span
        >
      </router-link>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { getSupabase } from "../supabase/init";
export default {
  name: "home",
  components: {},
  setup() {
    // Create variables:
    const data = ref([]);
    const dataLoaded = ref(null);

    // Get Data:
    const getData = async () => {
      try {
        const { data: workouts, error } = await getSupabase()
          .from("workouts")
          .select("*");
        if (error) throw error;
        data.value = workouts;
        dataLoaded.value = true;
      } catch (error) {
        console.warn(error.message);
      }
    };

    getData();

    // Run the data function:

    return { data, dataLoaded };
  },
};
</script>

<style></style>
