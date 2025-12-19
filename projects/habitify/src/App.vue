<script setup lang="ts">
import { ref } from "vue";
const habit = ref();
const error = ref("");

const habits = ref<string[]>(["habit"]);

// add a habit to the habits array if input is > 10 chars
// throw an error if not
function addHabit() {
  if (habit.value.length > 5) {
    habits.value.push(habit.value);
    error.value = "";
    habit.value = "";
  } else {
    habit.value = habit.value;
    error.value = "Your habit should not be less than 10 characters in length";
  }
}

// remove a habit from an array
function deleteHabit(id: number) {
  habits.value.splice(id, 1);
  return habits.value;
}

// make a habit
</script>

<template>
  <div class="py-16 flex justify-center">
    <div class="max-h-[500] w-1/3 bg-white">
      <h1 class="">Welcome to Habitify</h1>
      <div>
        <input
          v-model="habit"
          placeholder="Input a habit to keep track of it..."
        />
        <button @click="addHabit" class="cursor-pointer">
          <i class="pi pi-plus"></i>
        </button>
      </div>

      <table class="w-full border">
        <th class="flex border-b">
          <tr class="w-2/3">
            Habit
          </tr>
          <tr class="border-l">
            Actions
          </tr>
        </th>

        <tbody>
          <div v-for="(habit, index) in habits">
            <div class="border-b flex">
              <tr class="w-2/3 pl-6">
                {{
                  habit.charAt(0).toUpperCase() + habit.slice(1)
                }}
              </tr>
              <tr class="border-l">
                <button @click="deleteHabit(index)">
                  <i class="pi pi-trash"></i>
                </button>
                <button><i class="pi pi-check"></i></button>
              </tr>
            </div>
          </div>
        </tbody>
      </table>
    </div>
  </div>
</template>
