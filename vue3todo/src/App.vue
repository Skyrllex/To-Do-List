<template>
  <main>
    <button @click="toggleModal">{{ showModal ? 'Скрыть' : 'Добавить задачу' }}</button>
    <TaskInput v-if="showModal" @onAddTask="addTask"></TaskInput>
    <ul class="task-list">
      <li v-for="item in taskList" :key="item.id">
       <ToDoList 
         :model="item" 
         @onRemove="setRemoveTask(item.id)" 
         @onDone="setDoneTask(item.id)" 
         @onEdit="setEditTask(item.id, $event.title, $event.description)">
       </ToDoList>
      </li>
    </ul>
  </main>
</template>

<script>
import ToDoList from './components/ToDoList.vue';
import TaskInput from './components/TaskInput.vue';
import { ref, onMounted } from 'vue';

export default {
  name: 'App',
  components: {
    TaskInput,
    ToDoList,
  },

  setup() {
    const showModal = ref(false);
    const taskList = ref([]);

    // Функция для загрузки задач из localStorage
    const loadTasks = () => {
      const tasks = localStorage.getItem('tasks');
      if (tasks) {
        taskList.value = JSON.parse(tasks);
      } else {
        taskList.value = [{ id: 0, title: 'Добро пожаловать в To-do-List', description: 'Описание задачи', status: false }];
      }
    };

    // Функция для сохранения задач в localStorage
    const saveTasks = () => {
      localStorage.setItem('tasks', JSON.stringify(taskList.value));
    };

    // Загружаем задачи при монтировании компонента
    onMounted(() => {
      loadTasks();
    });

    const toggleModal = () => {
      showModal.value = !showModal.value;
    };

    const addTask = ({ title, description }) => {
      const newTask = {
        id: taskList.value.length > 0 ? taskList.value[taskList.value.length - 1].id + 1 : 0,
        title,
        description,
        status: false,
      };
      taskList.value.push(newTask);
      saveTasks(); // Сохраняем изменения
      showModal.value = false;
    };

    const setDoneTask = (id) => {
      const task = taskList.value.find(x => x.id === id);
      if (task) {
        task.status = !task.status; // Переключаем статус
        saveTasks(); // Сохраняем изменения
      }
    };

    const setEditTask = (id, newTitle, newText) => {
      const task = taskList.value.find(x => x.id === id);
      if (task) {
        task.title = newTitle;
        task.description = newText;
        saveTasks(); // Сохраняем изменения
      }
    };

    const setRemoveTask = (id) => {
      taskList.value = taskList.value.filter(x => x.id !== id);
      saveTasks(); // Сохраняем изменения
    };

    return {
      taskList,
      addTask,
      setRemoveTask,
      setDoneTask,
      setEditTask,
      toggleModal,
      showModal,
    };
  },
};
</script>

<style scoped>
.task-list {
  list-style: none;
  text-align: justify;
}
button {
  margin: 0 auto;
  display: block;
  margin-top: 10px;
}
</style>
