<template>
  <div>
    <ul>
      <!-- Loop through tasks and display each task -->
      <li v-for="task in tasks" :key="task.id" class="mb-4 p-4 border rounded-xl flex justify-between items-center dark:bg-slate-800 dark:border-gray-700 dark:text-white shadow-lg w-full">
        <div>
          <h3 class="font-bold">{{ task.description }}</h3>
          <p class="dark:text-gray-300 text-md">{{ task.startDate }} - {{ task.endDate }}</p>
          <p>État : {{ task.status }}</p>
          <p>Priorité : {{ task.priority }}</p>
        </div>
        <div class="flex space-x-4">
          <!-- Edit Task Button -->
          <button @click="editTask(task)" class="hover:bg-sky-500 hover:bg-opacity-25 text-sky-400 font-semibold hover:text-sky-500 dark:hover:text-white py-2 px-4 border dark:border-slate-700 hover:border-sky-500 dark:hover:border-sky-500 rounded-lg hover:shadow-lg shadow-sky-500 transition">
            Modifier
          </button>
          <!-- Remove Task Button -->
          <button @click="removeTask(task.id)" class="hover:bg-red-500 hover:bg-opacity-25 text-red-400 font-semibold hover:text-red-500 dark:hover:text-white py-2 px-4 border dark:border-slate-700 hover:border-red-500 dark:hover:border-red-500 rounded-lg hover:shadow-lg shadow-red-500 transition">
            Supprimer
          </button>
        </div>
      </li>
      <!-- Display message if there are no tasks -->
      <li v-if="tasks.length === 0" class="p-4 border rounded-xl dark:bg-slate-800 dark:border-gray-700 dark:text-white shadow-md">
        Aucune tâche à afficher
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    // Receive tasks as a prop from the parent component
    tasks: {
      type: Array,
      required: true
    }
  },
  methods: {
    // Emit event to edit task
    editTask(task) {
      this.$emit('edit-task', task);
    },
    // Emit event to remove task
    removeTask(id) {
      this.$emit('remove-task', id);
    }
  }
}
</script>
