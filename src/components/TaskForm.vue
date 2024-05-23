<template>
  <form @submit.prevent="addTask" class="absolute top-0 left-0 w-full h-full bg-black bg-opacity-50 flex justify-center items-center">
    <div class="w-1/2 space-y-4 p-4 border rounded-3xl dark:bg-slate-800 border-gray-300 dark:border-gray-700 shadow-lg shadow-slate-800">
      <div>
        <label for="description" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Description</label>
        <textarea v-model="task.description" id="description" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white" required></textarea>
      </div>
      <div>
        <label for="startDate" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Date de début</label>
        <input type="date" v-model="task.startDate" id="startDate" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white" required>
      </div>
      <div>
        <label for="endDate" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Date de fin</label>
        <input type="date" v-model="task.endDate" id="endDate" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white" required>
      </div>
      <div>
        <label for="status" class="block text-sm font-medium text-gray-700 dark:text-gray-300">État</label>
        <select v-model="task.status" id="status" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white" required>
          <option value="à faire">À faire</option>
          <option value="en cours">En cours</option>
          <option value="terminé">Terminé</option>
        </select>
      </div>
      <div>
        <label for="priority" class="block text-sm font-medium text-gray-700 dark:text-gray-400">Priorité</label>
        <select v-model="task.priority" id="priority" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white" required>
          <option value="haute">Haute</option>
          <option value="moyenne">Moyenne</option>
          <option value="basse">Basse</option>
        </select>
      </div>
      <div class="flex justify-between">
        <button type="button" @click="$emit('cancel')" class="hover:bg-red-500 hover:bg-opacity-25 text-red-400 font-semibold hover:text-white py-2 px-4 border border-slate-700 hover:border-red-500 rounded-lg hover:shadow-lg shadow-red-500">Annuler</button>
        <button type="submit" class="hover:bg-sky-500 hover:bg-opacity-25 text-sky-400 font-semibold hover:text-white py-2 px-4 border border-slate-700 hover:border-sky-500 rounded-lg hover:shadow-lg shadow-sky-500">Ajouter</button>
      </div>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      task: {
        description: '',
        startDate: '',
        endDate: '',
        status: 'à faire',
        priority: 'moyenne'
      }
    };
  },
  methods: {
    addTask() {
      this.$emit('add-task', { ...this.task });
      this.resetForm();
    },
    resetForm() {
      this.task.description = '';
      this.task.startDate = '';
      this.task.endDate = '';
      this.task.status = 'à faire';
      this.task.priority = 'moyenne';
    }
  }
}
</script>

<style>
input[type="date"]::-webkit-calendar-picker-indicator {
  filter: invert(1);
}
</style>
