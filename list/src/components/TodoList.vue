<template>
  <div class="todo-list">
    <h2>Todo List</h2>
    <input class="new-todo-input" v-model="newTodo" placeholder="Add a new task">
    <button class="add-todo-button" @click="addTodo">Add</button>

    <ul class="todo-items">
      <li v-for="todo in todos" :key="todo.id" class="todo-item">
        <input type="checkbox" v-model="todo.completed" class="todo-checkbox">
        <span :class="{ completed: todo.completed, editing: todo.editing }" class="todo-title">
          <span v-if="!todo.editing">{{ todo.title }}</span>
          <input v-else v-model="todo.updatedTitle" @keyup.enter="saveTodo(todo)" @blur="cancelEdit(todo)" class="edit-todo-input">
        </span>
        <button @click="deleteTodo(todo.id)" class="delete-todo-button">Delete</button>
        <button v-if="!todo.editing" @click="startEdit(todo)" class="edit-todo-button">Edit</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      newTodo: ''
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push({
          id: Date.now(),
          title: this.newTodo,
          completed: false,
          editing: false,
          updatedTitle: ''
        });
        this.newTodo = '';
      }
    },
    deleteTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    startEdit(todo) {
      todo.editing = true;
      todo.updatedTitle = todo.title;
    },
    saveTodo(todo) {
      if (todo.updatedTitle.trim() !== '') {
        todo.title = todo.updatedTitle;
        todo.editing = false;
      }
    },
    cancelEdit(todo) {
      todo.editing = false;
    }
  }
};
</script>

<style>
.todo-list {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f8f8f8;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h2 {
  font-size: 24px;
  margin-bottom: 16px;
}

.new-todo-input {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
}

.add-todo-button {
  padding: 8px 16px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 16px;
}

.todo-items {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.todo-checkbox {
  margin-right: 8px;
}

.todo-title {
  flex-grow: 1;
  font-size: 16px;
}

.completed {
  text-decoration: line-through;
}

.editing .todo-title {
  display: none;
}

.edit-todo-input {
  width: 80%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 16px;
  margin-left: 8px;
}
.edit-todo-button {
  margin-left: 4px;
}

.delete-todo-button,
.edit-todo-button {
  padding: 4px 8px;
  background-color: #f44336;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
  margin:0px 3px 10px 0px;
}
</style>
