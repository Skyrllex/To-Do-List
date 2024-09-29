<template>
    <div class = "task_input my-list">
        <input v-model= "title" placeholder="Введите название" type="text">
        <textarea v-model= "description"  placeholder="Введите описание" class="input2"></textarea>
        <button @click="onAddTask">Добавить задачу</button>
    </div>
</template>


<script>
import { ref } from 'vue'

export default {
    emits: {
        onAddTask({title, description}){
        }
    },
    setup (props,{emit}) 
    {
        const title = ref('')
        const description = ref('')
   
        const onAddTask = () => {
            if (title.value === '' || description.value === '') {
                alert('Пожалуйста, заполните название и описание задачи!');
                return;
            }

            emit('onAddTask', { title: title.value, description: description.value });
            console.log('emit', { title: title.value, description: description.value });

            // Очистка полей после добавления задачи
            title.value = '';
            description.value = '';
        };

        return {
            title,
            description,
            onAddTask
        }
    }
}
</script>

<style scoped>
.task_input{
    margin-top: 10px;
}
</style>