<template>
  <div class="tasks-container">
    <table>
        <tr>
            <th scope="col">Id</th>
            <th scope="col">Description</th>
            <th scope="col">State</th>
            <th scope="col">Actions</th>
        </tr>
        <tr v-for="task in tasks" :key="task.idTask">
            <td>{{ task.idTask }}</td>
            <td>{{ task.descTask }}</td>
            <td><button class="state-btn" @click="completeTask(task.idTask)">{{ task.stateTask }}</button></td>
            <td><button class="delete-btn" @click="deleteTask(task.idTask)">Delete</button></td>
        </tr>
    </table>
    </div>
    <div class="add-task">
        <input v-model="newTaskDescription" type="text" placeholder="enter task description"/>
        <button @click="addTask">add Task</button>
    </div>
</template>

<script>
import api from '../services/api';

export default {
  name: 'Show_tasks_list',
  data() {
    return {
      tasks: [],
      newTaskDescription: '',
    };
  },
  async created() {
    try {
      // Appel à l'API pour récupérer le titre
      const response = await api.getShowTasks();
      this.tasks = response.data || response; // Mettez à jour la propriété avec le résultat
    } catch (error) {
      console.error('Erreur lors de la récupération des tasks :', error);
    }
  },
  methods: {
  async deleteTask(idTask){
    try{
        await api.deleteTask(idTask);
        this.tasks = this.tasks.filter(task => task.idTask !== idTask);
    } catch (error){
        console.error('Erreur dans la suppression de la tâche :', error);
    }
  },
    async completeTask(idTask){
        try{
            await api.completeTask(idTask);
            const task = this.tasks.find(t => t.idTask === idTask);
            if (task){
                task.stateTask= 'OVER';
            }
        } catch (error){
            console.error('Erreur dans la mise à jour de la tâche :', error);
        }
    },
    async addTask(){
        try{
            const response = await api.addTask(this.newTaskDescription);
            this.tasks.push(response.data);
            this.newTaskDescription='';
        } catch (error){
            console.error('La tâche a pas pu être ajouté :', error);
        }
    }
}
};
</script>

<style scoped>
.tasks-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 400px;
  padding: 20px;
}

table {
  border-collapse: collapse;
  width: 80%;
  max-width: 800px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

th, td {
  padding: 12px 15px;
  text-align: left;
  border-bottom: 1px solid #ddd;
}

th {
  background-color: #2c3e50;
  color: white;
  font-weight: bold;
}

tr:hover {
  background-color: #f5f5f5;
}

.state-btn {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  background-color: #3498db;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.state-btn:hover {
  background-color: #2980b9;
}

.delete-btn {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  background-color: #e74c3c;
  color: white;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.delete-btn:hover {
  background-color: #c0392b;
}
</style>