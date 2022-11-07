<template>
  <div class="max-w-screen-md mx-auto px-4 py-10">
    <!-- STATUS MESSAGE -->
    <div
      v-if="statusMsg || errorMsg"
      class="mb-10 p-4 bg-light-grey rounded-md"
    >
      <span class="text-at-light-green">{{ statusMsg }}</span>
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- CREATE -->
    <div class="p-8 flex items-start bg-light-grey rounded-md shadow-lg">
      <!-- FORM -->
      <form
        @submit.prevent="createWorkout"
        class="flex flex-col gap-y-5 w-full"
      >
        <h2 class="text-2xl text-center font-bold text-at-light-green">
          Record Workout
        </h2>

        <!-- WORKOUT NAME -->
        <div class="flex flex-col">
          <label for="workout-name" class="mb-1 text-sm text-at-light-green">
            Workout Name
          </label>
          <input
            type="text"
            name="workout-name"
            id="workout-name"
            required
            v-model="workoutName"
            class="p-2 text-gray-500 focus:outline-none"
          />
        </div>

        <!-- WORKOUT TYPE -->
        <div class="flex flex-col">
          <label for="workout-type" class="mb-1 text-sm text-at-light-green">
            Workout Type
          </label>
          <select
            name="workout-type"
            id="workout-type"
            class="p-2 text-gray focus:outline-none"
            @change="workoutChange"
            v-model="workoutType"
          >
            <option value="select-workout" disabled hidden>
              Select Workout
            </option>
            <option value="strength">Strength</option>
            <option value="cardio">Cardio</option>
          </select>
        </div>

        <!-- STRENGTH TRAINING -->
        <div v-if="workoutType === 'strength'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label
                for="exercise-name"
                class="mb-1 text-sm text-at-light-green"
                >Exercises</label
              >
              <input
                type="text"
                name="exercise-name"
                id="exercise-name"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.exercise"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="sets" class="mb-1 text-sm text-at-light-green"
                >Sets</label
              >
              <input
                type="text"
                name="sets"
                id="sets"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.sets"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="reps" class="mb-1 text-sm text-at-light-green"
                >Reps</label
              >
              <input
                type="text"
                name="reps"
                id="reps"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.reps"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="weight" class="mb-1 text-sm text-at-light-green"
                >Weight, kg</label
              >
              <input
                type="text"
                name="weight"
                id="weight"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.weight"
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              src="../assets/images/trash-light-green.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
            />
          </div>

          <button
            @click="addExercise"
            type="button"
            class="mt-6 py-2 px-6 rounded-md self-center text-base font-medium text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Add Exercise
          </button>
        </div>

        <!-- CARDIO TRAINING -->
        <div v-if="workoutType === 'cardio'" class="flex flex-col gap-y-4">
          <div
            class="flex flex-col gap-x-6 gap-y-2 relative md:flex-row"
            v-for="(item, index) in exercises"
            :key="index"
          >
            <div class="flex flex-col md:w-1/3">
              <label for="cardio-type" class="mb-1 text-sm text-at-light-green"
                >Type</label
              >
              <select
                name="cardio-type"
                id="cardio-type"
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.cardioType"
              >
                <option value="select-cardio" disabled hidden>
                  Select Type
                </option>
                <option value="run">Run</option>
                <option value="walk">Walk</option>
              </select>
            </div>
            <div class="flex flex-col flex-1">
              <label for="distance" class="mb-1 text-sm text-at-light-green"
                >Distance, km</label
              >
              <input
                type="number"
                name="distance"
                id="distance"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.distance"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="duration" class="mb-1 text-sm text-at-light-green"
                >Duration</label
              >
              <input
                type="text"
                name="duration"
                id="duration"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.duration"
              />
            </div>
            <div class="flex flex-col flex-1">
              <label for="pace" class="mb-1 text-sm text-at-light-green"
                >Pace</label
              >
              <input
                type="text"
                name="pace"
                id="pace"
                required
                class="p-2 w-full text-gray-500 focus:outline-none"
                v-model="item.pace"
              />
            </div>
            <img
              @click="deleteExercise(item.id)"
              src="../assets/images/trash-light-green.png"
              class="h-4 w-auto absolute -left-5 cursor-pointer"
            />
          </div>

          <button
            @click="addExercise"
            type="button"
            class="mt-6 py-2 px-6 rounded-md self-center text-base font-medium text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
          >
            Add Exercise
          </button>
        </div>

        <hr class="y-4" />

        <button
          type="submit"
          class="mt-6 py-2 px-6 rounded-md self-center text-base font-medium text-white bg-at-light-green duration-200 border-solid border-2 border-transparent hover:border-at-light-green hover:bg-white hover:text-at-light-green"
        >
          Record Workout
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";
import { uid } from "uid";
import { getSupabase } from "../supabase/init";
export default {
  name: "create",
  setup() {
    // Create variables:
    const workoutName = ref("");
    const workoutType = ref("select-workout");
    const exercises = ref([]);
    const statusMsg = ref(null);
    const errorMsg = ref(null);

    // Add Exercises:
    const addExercise = () => {
      if (workoutType.value === "strength") {
        exercises.value.push({
          id: uid(),
          exercises: "",
          sets: "",
          reps: "",
          weight: "",
        });

        return;
      }

      if (workoutType.value === "cardio") {
        exercises.value.push({
          id: uid(),
          cardioType: "",
          distance: null,
          duration: "",
          pace: "",
        });

        return;
      }
    };

    // Delete Exercises:
    const deleteExercise = (id) => {
      if (exercises.value.length > 1) {
        exercises.value = exercises.value.filter(
          (exercise) => exercise.id !== id
        );
        return;
      }

      errorMsg.value =
        "Error: cannot remove exercise, need at least one exercise to delete";
      setTimeout(() => {
        errorMsg.value = false;
      }, 5000);
    };

    // Listen for changes:
    const workoutChange = () => {
      exercises.value = [];
      addExercise();
    };

    // Create a workout:
    const createWorkout = async () => {
      try {
        const { error } = await getSupabase()
          .from("workouts")
          .insert([
            {
              workoutName: workoutName.value,
              workoutType: workoutType.value,
              exercises: exercises.value,
            },
          ]);
        if (error) throw error;
        statusMsg.value = "Successfully created a new workout";
        workoutName.value = null;
        workoutType.value = "select-workout";
        exercises.value = [];

        setTimeout(() => {
          statusMsg.value = false;
        }, 5000);
      } catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
          errorMsg.value = false;
        }, 5000);
      }
    };

    return {
      workoutName,
      workoutType,
      exercises,
      statusMsg,
      errorMsg,
      addExercise,
      deleteExercise,
      workoutChange,
      createWorkout,
    };
  },
};
</script>

<style></style>
