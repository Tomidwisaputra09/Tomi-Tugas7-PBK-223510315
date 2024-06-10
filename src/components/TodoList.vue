<template>
  <div class="todo-list">
    <h1>LIST TUGAS</h1>
    <div class="input-container">
      <input
        v-model="newTodo"
        placeholder="Add a new task"
        @keyup.enter="addTodo"
      />
      <button @click="addTodo">Add</button>
    </div>
    <ul>
      <li v-for="(todo, index) in todos" :key="index">
        <span
          :class="{ completed: todo.completed }"
          @click="toggleTodoCompletion(index)"
        >
          {{ todo.text }}
        </span>
        <button @click="deleteTodo(index)">Delete</button>
      </li>
    </ul>
    <p>{{ incompleteTodos }} tasks remaining</p>
  </div>
</template>

<script>
import { useTodoStore } from "../stores/todoStore";
import { computed, ref } from "vue";

export default {
  setup() {
    const store = useTodoStore();
    const newTodo = ref("");

    const addTodo = () => {
      if (newTodo.value.trim()) {
        store.addTask(newTodo.value);
        newTodo.value = "";
      }
    };

    const deleteTodo = (index) => {
      store.removeTask(index);
    };

    const toggleTodoCompletion = (index) => {
      store.toggleTaskStatus(index);
    };

    return {
      newTodo,
      todos: computed(() => store.tasks),
      addTodo,
      deleteTodo,
      toggleTodoCompletion,
      incompleteTodos: computed(() => store.incompleteTasksCount),
    };
  },
};
</script>

<style scoped>
.todo-list {
  max-width: 500px;
  margin: 60px auto;
  padding: 30px;
  border-radius: 15px;
  box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
  background: linear-gradient(135deg, #c60e1d 0%, #eda949 100%);
  font-family: "Roboto", sans-serif;
  color: #ffffff;
  text-align: center;
}

h1 {
  color: #fff;
  font-size: 2.5em;
  margin-bottom: 20px;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.input-container {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

input {
  width: calc(100% - 90px);
  padding: 12px;
  margin-right: 10px;
  border: 2px solid #fff;
  border-radius: 25px;
  background: rgba(255, 255, 255, 0.2);
  color: #fff;
  transition: background 0.3s ease;
}

input:focus {
  background: rgba(252, 252, 252, 0.5);
  outline: none;
}
input::placeholder {
  color: #ffffff; /* Ubah warna teks placeholder di sini */
}

button {
  padding: 12px 20px;
  border: none;
  border-radius: 25px;
  background-color: #b6065e;
  color: #fff;
  cursor: pointer;
  transition: background 0.3s ease;
}

button:hover {
  background-color: #ff4e50;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px solid rgba(255, 255, 255, 0.3);
  transition: background 0.3s ease;
}

li:hover {
  background: rgba(255, 255, 255, 0.1);
}

li .completed {
  text-decoration: line-through;
  color: #d3d3d3;
}

span {
  cursor: pointer;
  flex-grow: 1;
}

span:hover {
  text-decoration: underline;
}

p {
  margin-top: 20px;
  font-size: 1.2em;
  color: #fff;
}
</style>
