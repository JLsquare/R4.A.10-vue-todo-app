<template>
  <div class="w-screen min-h-screen dark:bg-slate-900 p-8 relative">
    <h1 class="text-4xl font-bold mb-6 dark:text-white">ToDo</h1>
    <button @click="showForm = true" class="mb-4 block hover:bg-sky-500 hover:bg-opacity-25 text-sky-400 font-semibold hover:text-white py-2 px-4 border border-slate-700 hover:border-sky-500 rounded-lg hover:shadow-lg shadow-sky-500 transition">
      Ajouter une tâche
    </button>
    <task-form v-if="showForm" @add-task="addTask" @cancel="showForm = false" />
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 my-4">
      <div class="filter-item transform transition duration-300 hover:-translate-y-1 hover:shadow-lg">
        <label for="statusFilter" class="block text-sm font-medium text-gray-700 dark:text-gray-300">État</label>
        <select v-model="statusFilter" id="statusFilter" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white">
          <option value="">Tous</option>
          <option value="à faire">À faire</option>
          <option value="en cours">En cours</option>
          <option value="terminé">Terminé</option>
        </select>
      </div>
      <div class="filter-item transform transition duration-300 hover:-translate-y-1 hover:shadow-lg">
        <label for="priorityFilter" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Priorité</label>
        <select v-model="priorityFilter" id="priorityFilter" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white">
          <option value="">Tous</option>
          <option value="haute">Haute</option>
          <option value="moyenne">Moyenne</option>
          <option value="basse">Basse</option>
        </select>
      </div>
      <div class="filter-item transform transition duration-300 hover:-translate-y-1 hover:shadow-lg">
        <label for="startDateFilter" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Date de début</label>
        <input type="date" v-model="startDateFilter" id="startDateFilter" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white">
      </div>
      <div class="filter-item transform transition duration-300 hover:-translate-y-1 hover:shadow-lg">
        <label for="endDateFilter" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Date de fin</label>
        <input type="date" v-model="endDateFilter" id="endDateFilter" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white">
      </div>
    </div>
    <task-list :tasks="filteredTasks" @remove-task="removeTask" />
  </div>
</template>

<script>
import TaskForm from './components/TaskForm.vue';
import TaskList from './components/TaskList.vue';

export default {
  components: {
    TaskForm,
    TaskList
  },
  data() {
    return {
      tasks: JSON.parse(localStorage.getItem('tasks')) || [],
      statusFilter: '',
      priorityFilter: '',
      startDateFilter: '',
      endDateFilter: '',
      showForm: false
    };
  },
  computed: {
    filteredTasks() {
      return this.tasks.filter(task => {
        return (!this.statusFilter || task.status === this.statusFilter) &&
               (!this.priorityFilter || task.priority === this.priorityFilter) &&
               (!this.startDateFilter || new Date(task.startDate) >= new Date(this.startDateFilter)) &&
               (!this.endDateFilter || new Date(task.endDate) <= new Date(this.endDateFilter));
      });
    }
  },
  methods: {
    addTask(task) {
      task.id = Date.now();
      this.tasks.push(task);
      this.saveTasks();
      this.showForm = false;
    },
    removeTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
      this.saveTasks();
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    }
  },
  watch: {
    tasks: {
      handler() {
        this.saveTasks();
      },
      deep: true
    }
  }
}
</script>
