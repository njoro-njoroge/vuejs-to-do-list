<template>
  <div class="main-content">
    <h1>To-Do List</h1>
    <div class="card">
      <p v-if="message" class="message">{{ message }}</p>
      <input
        type="text"
        v-model="todo"
        placeholder="Enter a new to-do item"
        class="todo-input"
      />
      <button
        type="button"
        @click="handleSubmit"
        class="add-button"
      >
        Add
      </button>
      <br/>
      <br/>
      <br/>
      <div v-if="data.length" class="todo-list">
        <p>{{ countTodo }} Task</p>
        <ol>
          <li v-for="(item, index) in data" :key="index">
            <div class="todo-item">
              {{ index + 1 }}
              <input 
                v-if="isEditing === index" 
                type="text" 
                v-model="editText" 
                class="edit-todo-input"
              />
              <input 
                v-else 
                type="checkbox" 
                class="done" 
                id="done-{{index}}" 
                v-model="item.done" 
                @click="handleCheck(index)" 
              />
              <label 
                v-if="isEditing !== index" 
                :for="'done-' + index" 
                :class="{ 'strikethrough': item.done }" 
                class="todo-label"
              >
                {{ item.text }}
              </label>
            </div>
            <div class="btn-group">
              <button 
                v-if="isEditing === index" 
                @click="saveEdit(index)" 
                class="btn-save"
              >
                Save
              </button>
              <button 
                v-else 
                @click="startEditing(index, item.text)" 
                class="btn-update"
              >
                Update
              </button>
              <button 
                @click="handleRemove(index)" 
                class="btn-remove"
              >
                Remove
              </button>
            </div>
          </li>
        </ol>
      </div>
      <p v-else class="empty-list">No to-dos yet. Add one above!</p>
    </div>
  </div>
</template>


  
<script>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const data = ref([]);
    const countTodo = ref(0);
    const todo = ref('');
    const message = ref('');
    const isEditing = ref(null);  // Track the index of the item being edited
    const editText = ref('');      // Store the text being edited

    const fetchToDo = () => {
      const storedTodos = localStorage.getItem('todo');
      if (storedTodos) {
        data.value = JSON.parse(storedTodos);
        countTodo.value = data.value.length;    
      } else {
        message.value = 'No to-do records found.';
      }
    };

    const handleSubmit = () => {
      if (todo.value.trim()) {
        data.value.push({ text: todo.value, done: false });
        localStorage.setItem('todo', JSON.stringify(data.value));
        countTodo.value += 1;
        todo.value = '';
        message.value = '';
      } else {
        message.value = 'Please enter a value.';
      }
    };

    const handleCheck = (index) => {
      data.value[index].done = !data.value[index].done;
      localStorage.setItem('todo', JSON.stringify(data.value));
    };

    const startEditing = (index, text) => {
      isEditing.value = index;
      editText.value = text;
    };

    const saveEdit = (index) => {
      if(editText.value===''){
        message.value = 'Input value is empty';
        return;
      }
      data.value[index].text = editText.value;
      localStorage.setItem('todo', JSON.stringify(data.value));
      isEditing.value = null;
      message.value="Updated successfully";
    };

    const handleRemove = (index) => {
      data.value.splice(index, 1);
      localStorage.setItem('todo', JSON.stringify(data.value));
      countTodo.value -=1;
    };

    onMounted(() => {
      fetchToDo();
    });

    return {
      data,
      message,
      todo,
      countTodo,
      isEditing,
      editText,
      handleSubmit,
      handleCheck,
      startEditing,
      saveEdit,
      handleRemove,
    };
  }
};

</script>

<style scoped>
.main-content {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

.card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  background-color: #fff;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

.message {
  color: #d9534f;
  margin-bottom: 10px;
}

.todo-input {
  width: 100%;
  width: calc(100% - 120px);
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.edit-todo-input {
  width: auto !important;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.add-button {
  padding: 10px 15px;
  border: none;
  border-radius: 4px;
  background-color: #5bc0de;
  color: #fff;
  cursor: pointer;
  margin-left: 10px;
}

.add-button:hover {
  background-color: #31b0d5;
}

.todo-list ol {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.todo-list li {
  padding: 8px 0;
  border-bottom: 1px solid #ddd;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.todo-item {
  display: flex;
  align-items: center;
}

.todo-item .done {
  margin-right: 10px;
}

.btn-group {
  display: flex;
  gap: 10px;
}

.btn-save {
  padding: 7px 10px;
  border: none;
  border-radius: 4px;
  background-color: hsl(155, 86%, 57%);
  color: #fff;
  font-size: 12px;
  cursor: pointer;
}
.btn-update {
  padding: 7px 10px;
  border: none;
  border-radius: 4px;
  background-color: hsl(214, 86%, 57%);
  color: #fff;
  font-size: 12px;
  cursor: pointer;
}

.btn-remove {
  padding: 7px 10px;
  border: none;
  border-radius: 4px;
  background-color: #de5b7e;
  color: #fff;
  font-size: 12px;
  cursor: pointer;
}

.strikethrough {
  text-decoration: line-through;
  color: #999;
}

.empty-list {
  color: #999;
}
</style>



