<template>
  <div class="todo-container">
    <h1>To-Do List</h1>
    <div class="input-section">
      <!-- Input for adding new tasks -->
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Add a new task"
      />
      <button @click="addTask">Add</button>
    </div>

    <!-- List of tasks -->
    <ul class="task-list">
      <li v-for="(task, index) in tasks" :key="index" :class="{ completed: task.completed }">
        <!-- Editing Mode -->
        <div v-if="editIndex === index">
          <input
            v-model="editText"
            @keyup.enter="saveEdit(index)"
            placeholder="Edit task"
          />
          <div class="button-container">
            <button @click="saveEdit(index)" class="edit">Save</button>
            <button @click="cancelEdit" class="delete">Cancel</button>
          </div>
        </div>

        <!-- View Mode -->
        <div v-else>
          <span @click="toggleComplete(index)">{{ task.text }}</span>
          <div class="button-container">
            <button @click="startEdit(index, task.text)" class="edit">Edit</button>
            <button @click="removeTask(index)" class="delete">Delete</button>
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>


<script setup>
import { ref } from 'vue';

// Tasks list (array) and new task input (reactive variables)
const tasks = ref([]);
const newTask = ref('');

// Variables for editing functionality
const editIndex = ref(null); // Tracks the task being edited
const editText = ref(''); // Holds the temporary text for editing

// Function to add a task
const addTask = () => {
  if (newTask.value.trim()) {
    tasks.value.push({ text: newTask.value, completed: false });
    newTask.value = '';
  }
};

// Function to toggle task completion
const toggleComplete = (index) => {
  tasks.value[index].completed = !tasks.value[index].completed;
};

// Function to remove a task
const removeTask = (index) => {
  tasks.value.splice(index, 1);
};

// Function to start editing a task
const startEdit = (index, currentText) => {
  editIndex.value = index; // Set the index of the task being edited
  editText.value = currentText; // Set the current text of the task into the edit input
};

// Function to save edited task
const saveEdit = (index) => {
  if (editText.value.trim()) {
    tasks.value[index].text = editText.value; // Update the task text
    editIndex.value = null; // Exit editing mode
    editText.value = ''; // Clear the edit input
  }
};

// Function to cancel editing
const cancelEdit = () => {
  editIndex.value = null; // Exit editing mode
  editText.value = ''; // Clear the edit input
};
</script>


<style scoped>
.todo-container {
  max-width: 400px;
  margin: 2rem auto;
  background: #fff;
  padding: 1rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  margin-bottom: 1rem;
}

.input-section {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

input {
  flex: 1;
  padding: 0.5rem;
  margin-right: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 0.5rem 1rem;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

.task-list {
  list-style: none;
  padding: 0;
}

.task-list li {
  display: flex;
  align-items: center; /* Align text and buttons vertically */
  justify-content: space-between; /* Push text to the left and buttons to the right */
  padding: 0.5rem 0;
  border-bottom: 1px solid #eee;
}

.task-list li span {
  flex: 1; /* Make the text take up available space */
}

.task-list li.completed span {
  text-decoration: line-through;
  color: #999;
}

.task-list li .button-container {
  display: flex; /* Arrange buttons horizontally */
  gap: 0.5rem; /* Add space between buttons */
}

/* Edit/Save Button (Green) */
.task-list li .button-container .edit {
  background-color: #28a745; /* Green */
  color: #fff;
}

.task-list li .button-container .edit:hover {
  background-color: #218838; /* Darker green on hover */
}

/* Delete/Cancel Button (Red) */
.task-list li .button-container .delete {
  background-color: #dc3545; /* Red */
  color: #fff;
}

.task-list li .button-container .delete:hover {
  background-color: #c82333; /* Darker red on hover */
}
</style>

