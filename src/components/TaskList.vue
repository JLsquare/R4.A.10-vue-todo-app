<template>
  <div>
    <h2 class="text-2xl font-bold mb-4 dark:text-white">Liste des tâches</h2>
    <ul>
      <li v-for="task in tasks" :key="task.id" class="mb-4 p-4 border rounded-xl flex justify-between items-center dark:bg-slate-800 dark:border-gray-700 dark:text-white">
        <div>
          <h3 class="font-bold">{{ task.description }}</h3>
          <p class="dark:text-gray-300 text-md">{{ task.startDate }} - {{ task.endDate }}</p>
          <p>État : {{ task.status }}</p>
          <p>Priorité : {{ task.priority }}</p>
        </div>
        <button @click="removeTask(task.id)" class="hover:bg-red-500 hover:bg-opacity-25 text-red-400 font-semibold hover:text-white py-2 px-4 border border-slate-700 hover:border-red-500 rounded-lg">Supprimer</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: {
    tasks: {
      type: Array,
      required: true
    }
  },
  methods: {
    removeTask(id) {
      this.$emit('remove-task', id);
    }
  }
}
</script>
