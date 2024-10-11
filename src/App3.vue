<script setup>
import { onMounted, ref } from 'vue';

  const name = ref('John Doe');
  const status = ref('active');
  const tasks = ref(['task 1', 'task 2', 'task 3']);
  const newTask = ref('');
  const link = 'https://www.google.com';

  const toggleStatus = () => {
    if(status.value === 'active') {
        status.value = 'pending';
      } else if(status.value === 'pending') {
        status.value = 'inactive';
      } else {
       status.value = 'active';
      }
  };

  const addTask = () => {
    if(newTask.value.trim() !== '') {
      tasks.value.push(newTask.value);
      newTask.value = '';
    }
  };

  const deleteTask = (index) => {
    tasks.value.splice(index, 1);
  };

  onMounted(async () => {
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/todos');
      const data = await response.json();
      tasks.value = data.map((task) => task.title);
    } catch (error) {
      console.error('Error fetching tasks:', error);
    }
  });

</script>
<template>
 <h1>{{name}}</h1>
 <p v-if="status==='active'">User is active</p>
 <p v-else-if="status==='pending'">User is pending</p>
 <p v-else>User is inactive</p>

 <form @submit.prevent="addTask">
  <label for="newTask">Add Task</label>
  <input type="text" id="newTask" name="newTask" v-model="newTask" />
  <button type="submit">Add Task</button>
 </form>

 <h3>Tasks:</h3>
 <ul>
  <li v-for="(task, index) in tasks" :key="task">
    <button @click="deleteTask(index)">X</button>
    <span> {{task}} </span>
    
  </li>
 </ul>
 <!--<a v-bind:href="link">Google</a>-->
 <a :href="link">Click for Google</a>
 <br />
 <!--<button v-on:click="toggleStatus">Change Status</button>-->
 <button @click="toggleStatus">Change Users Status</button>
</template>

<style scoped>
  h1 {
    color: #2c3e50;
  }
</style>
