<template>
  <div class="w-screen min-h-screen bg-white dark:bg-slate-900 relative flex flex-col">
    <div class="p-8">
      <!-- Title -->
      <h1 class="text-4xl font-bold mb-6 dark:text-white">📋 Vue.js Todo List</h1>

      <!-- Action Buttons -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 my-4">
        <button @click="showAddForm = true" class="w-full block hover:bg-sky-500 hover:bg-opacity-25 text-sky-400 font-semibold hover:text-sky-500 dark:hover:text-white py-2 px-4 border dark:border-slate-700 hover:border-sky-500 dark:hover:border-sky-500 rounded-lg hover:shadow-lg shadow-sky-500 transition">
          Ajouter une tâche
        </button>
        <button @click="exportTasks" class="w-full block hover:bg-sky-500 hover:bg-opacity-25 text-sky-400 font-semibold hover:text-sky-500 dark:hover:text-white py-2 px-4 border dark:border-slate-700 hover:border-sky-500 dark:hover:border-sky-500 rounded-lg hover:shadow-lg shadow-sky-500 transition">
          Exporter mes tâches
        </button>
        <button @click="importTasks" class="w-full block hover:bg-sky-500 hover:bg-opacity-25 text-sky-400 font-semibold hover:text-sky-500 dark:hover:text-white py-2 px-4 border dark:border-slate-700 hover:border-sky-500 dark:hover:border-sky-500 rounded-lg hover:shadow-lg shadow-sky-500 transition">
          Importer des tâches
        </button>
        <button @click="toggleDarkMode" class="w-full block hover:bg-sky-500 hover:bg-opacity-25 text-sky-400 font-semibold hover:text-sky-500 dark:hover:text-white py-2 px-4 border dark:border-slate-700 hover:border-sky-500 dark:hover:border-sky-500 rounded-lg hover:shadow-lg shadow-sky-500 transition">
          {{ isDarkMode ? '☀️ Désactiver le mode sombre' : '🌙 Activer le mode sombre' }}
        </button>
      </div>

      <!-- Task Form -->
      <add-task-form v-if="showAddForm" @add-task="addTask" @cancel="showAddForm = false" />
      <edit-task-form v-if="showEditForm" :task-to-edit="taskToEdit" @update-task="updateTask" @cancel="showEditForm = false" />

      <!-- Filters -->
      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 my-4">
        <!-- Status Filter -->
        <div class="transform transition duration-300 hover:-translate-y-1 hover:drop-shadow-md dark:hover:drop-shadow-lg">
          <label for="statusFilter" class="block text-sm font-medium text-gray-700 dark:text-gray-300">État</label>
          <select v-model="statusFilter" id="statusFilter" class="mt-1 px-2 py-1 border rounded-lg w-full bg-white dark:bg-slate-900 dark:border-gray-700 dark:text-white">
            <option value="">Tous</option>
            <option value="à faire">À faire</option>
            <option value="en cours">En cours</option>
            <option value="terminé">Terminé</option>
          </select>
        </div>
        <!-- Priority Filter -->
        <div class="transform transition duration-300 hover:-translate-y-1 hover:drop-shadow-md dark:hover:drop-shadow-lg">
          <label for="priorityFilter" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Priorité</label>
          <select v-model="priorityFilter" id="priorityFilter" class="mt-1 px-2 py-1 border rounded-lg w-full bg-white dark:bg-slate-900 dark:border-gray-700 dark:text-white">
            <option value="">Tous</option>
            <option value="haute">Haute</option>
            <option value="moyenne">Moyenne</option>
            <option value="basse">Basse</option>
          </select>
        </div>
        <!-- Start Date Filter -->
        <div class="transform transition duration-300 hover:-translate-y-1 hover:drop-shadow-md dark:hover:drop-shadow-lg">
          <label for="startDateFilter" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Date de début</label>
          <input type="date" v-model="startDateFilter" id="startDateFilter" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white dark:[&::-webkit-calendar-picker-indicator]:invert">
        </div>
        <!-- End Date Filter -->
        <div class="transform transition duration-300 hover:-translate-y-1 hover:drop-shadow-md dark:hover:drop-shadow-lg">
          <label for="endDateFilter" class="block text-sm font-medium text-gray-700 dark:text-gray-300">Date de fin</label>
          <input type="date" v-model="endDateFilter" id="endDateFilter" class="mt-1 px-2 py-1 border rounded-lg w-full dark:bg-slate-900 dark:border-gray-700 dark:text-white dark:[&::-webkit-calendar-picker-indicator]:invert">
        </div>
      </div>

      <!-- Task List -->
      <task-list :tasks="filteredTasks" @edit-task="editTask" @remove-task="removeTask"/>
    </div>

    <!-- Hidden File Input for Import -->
    <input type="file" ref="fileInput" @change="handleFileUpload" style="display: none;" />

    <div class="flex-1"></div>
    <Footer />
  </div>
</template>

<script>
import AddTaskForm from './components/AddTaskForm.vue';
import EditTaskForm from './components/EditTaskForm.vue';
import TaskList from './components/TaskList.vue';
import Footer from './components/Footer.vue';

export default {
  components: {
    AddTaskForm,
    EditTaskForm,
    TaskList,
    Footer
  },
  data() {
    return {
      tasks: JSON.parse(localStorage.getItem('tasks')) || [],
      statusFilter: '',
      priorityFilter: '',
      startDateFilter: '',
      endDateFilter: '',
      showAddForm: false,
      showEditForm: false,
      taskToEdit: null,
      isDarkMode: JSON.parse(localStorage.getItem('darkMode')) || false
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
      // Generate a random ID for the task
      task.id = Math.random().toString(36).substr(2, 16);
      this.tasks.push(task);
      this.saveTasks();
      this.showForm = false;
    },
    editTask(task) {
      this.taskToEdit = task;
      this.showEditForm = true;
    },
    updateTask(updatedTask) {
      const index = this.tasks.findIndex(task => task.id === updatedTask.id);
      if (index !== -1) {
        this.tasks.splice(index, 1, updatedTask);
      }
      this.saveTasks();
      this.showEditForm = false;
    },
    removeTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
      this.saveTasks();
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode;
      document.documentElement.classList.toggle('dark', this.isDarkMode);
      localStorage.setItem('darkMode', this.isDarkMode);
    },
    exportTasks() {
      // Remove IDs from tasks before exporting
      const tasksWithoutIds = this.tasks.map(({ id, ...task }) => task);
      const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(tasksWithoutIds, null, 2));
      const downloadAnchorNode = document.createElement('a');
      downloadAnchorNode.setAttribute("href", dataStr);
      downloadAnchorNode.setAttribute("download", "tasks.json");
      document.body.appendChild(downloadAnchorNode);
      downloadAnchorNode.click();
      downloadAnchorNode.remove();
    },
    importTasks() {
      this.$refs.fileInput.click();
    },
    handleFileUpload(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        const importedTasks = JSON.parse(e.target.result);
        // Generate random IDs for the imported tasks
        importedTasks.forEach(task => task.id = Math.random().toString(36).substr(2, 16));
        this.tasks = [...this.tasks, ...importedTasks];
        this.saveTasks();
      };
      reader.readAsText(file);
    }
  },
  watch: {
    tasks: {
      handler() {
        this.saveTasks();
      },
      deep: true
    }
  },
  mounted() {
    document.documentElement.classList.toggle('dark', this.isDarkMode);
  }
}
</script>