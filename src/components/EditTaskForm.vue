<template>
  <div @click="$emit('cancel')" class="absolute top-0 left-0 z-10 w-full h-full bg-black bg-opacity-50 flex justify-center items-center">
    <form @submit.prevent="updateTask" @click.stop class="w-1/2 space-y-4 p-5 border rounded-3xl bg-white dark:bg-slate-800 border-gray-300 dark:border-gray-700 shadow-md dark:shadow-lg">
      <!-- Description -->
      <div>
        <label for="description" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Description</label>
        <textarea v-model="task.description" id="description" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white" required></textarea>
      </div>
      <!-- Start Date -->
      <div>
        <label for="startDate" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Date de début</label>
        <input type="date" v-model="task.startDate" id="startDate" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white dark:[&::-webkit-calendar-picker-indicator]:invert" required>
      </div>
      <!-- End Date -->
      <div>
        <label for="endDate" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Date de fin</label>
        <input type="date" v-model="task.endDate" id="endDate" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white dark:[&::-webkit-calendar-picker-indicator]:invert" required>
      </div>
      <!-- Status -->
      <div>
        <label for="status" class="block text-sm font-medium text-gray-700 dark:text-gray-300">État</label>
        <select v-model="task.status" id="status" class="mt-1 px-2 py-1 border rounded-lg w-full bg-white dark:bg-slate-900 dark:border-gray-700 dark:text-white" required>
          <option value="à faire">À faire</option>
          <option value="en cours">En cours</option>
          <option value="terminé">Terminé</option>
        </select>
      </div>
      <!-- Priority -->
      <div>
        <label for="priority" class="block text-sm font-medium text-gray-700 dark:text-gray-400">Priorité</label>
        <select v-model="task.priority" id="priority" class="mt-1 px-2 py-1 border rounded-lg w-full bg-white dark:bg-slate-900 dark:border-gray-700 dark:text-white" required>
          <option value="haute">Haute</option>
          <option value="moyenne">Moyenne</option>
          <option value="basse">Basse</option>
        </select>
      </div>
      <div class="flex justify-between">
        <!-- Cancel Button -->
        <button type="button" @click="$emit('cancel')" class="hover:bg-red-500 hover:bg-opacity-25 text-red-400 font-semibold hover:text-red-500 dark:hover:text-white py-2 px-4 border dark:border-slate-700 hover:border-red-500 dark:hover:border-red-500 rounded-lg hover:shadow-lg shadow-red-500 transition">
          Annuler
        </button>
        <!-- Submit Button -->
        <button type="submit" class="hover:bg-sky-500 hover:bg-opacity-25 text-sky-400 font-semibold hover:text-sky-500 dark:hover:text-white py-2 px-4 border dark:border-slate-700 hover:border-sky-500 dark:hover:border-sky-500 rounded-lg hover:shadow-lg shadow-sky-500 transition">
          Mettre à jour
        </button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: {
    taskToEdit: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      task: { ...this.taskToEdit }
    };
  },
  methods: {
    updateTask() {
      this.$emit('update-task', { ...this.task });
    }
  }
}
</script>