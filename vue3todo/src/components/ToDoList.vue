<template>
    <div :class="model.status ? 'one-task-style-true' : 'one-task-style'">
        <div id="text_to-do">
            <div :class="model.status ? 'h3_task-true' : 'h3_task'">
                <h3 v-if="!isEditing">{{ model.title }}</h3>
                <input v-if="isEditing" v-model="editedTitle" @keyup.enter="saveEdit" />
            </div>
            <div id="p_task">
                <p v-if="!isEditing">{{ model.description }}</p>
                <textarea v-if="isEditing" v-model="editedText" @keyup.enter="saveEdit" />
            </div>
        </div>

        <div>
            <button @click="emitOnDone">{{ model.status ? 'Отменить 🙌🏻' : 'Выполнить 💜' }}</button>
            <button @click="emitOnEdit">{{ isEditing ? 'Сохранить 👌' : 'Изменить 🖋' }}</button>
            <button @click="emitOnRemove">Удалить 🗑</button>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue';
export default {
    emits: ['onDone', 'onRemove', 'onEdit'],
    props: {
        model: {
            type: Object,
            required: true,
        },
    },
    setup(props, { emit }) {
        const isEditing = ref(false);
        const editedTitle = ref(props.model.title);
        const editedText = ref(props.model.description);

        const emitOnDone = () => {
            emit('onDone', props.model.id);
        };

        const emitOnRemove = () => {
            emit('onRemove', props.model.id);
        };

        const emitOnEdit = () => {
            if (isEditing.value) {
                    if (editedTitle.value === '' || editedText.value  === '') {
                    alert('Пожалуйста, заполните название и описание задачи!');
                    return;
                }
                emit('onEdit', { id: props.model.id, title: editedTitle.value, description: editedText.value });
            }
            isEditing.value = !isEditing.value;
        };

        const saveEdit = () => {
            emitOnEdit();
        };

        return {
            emitOnDone,
            emitOnRemove,
            emitOnEdit,
            saveEdit,
            isEditing,
            editedTitle,
            editedText,
        };
    },
};
</script>

<style scoped>
input{
    margin: 0 auto;
    width: 100%;
    height: auto;
    text-align: justify;
    background-color: rgb(0, 0, 0);
    color: #ffffff;
    border-radius: 20px;
    padding: 6px;
    margin-left: -10px;
    padding: 6px;
}
textarea{
    margin: 0 auto;
    width: 95%;
    height: fit-content;
    text-align: justify;   
    border-radius: 20px;
    padding: 4px;
    background-color: rgb(0, 0, 0);
    color: #ffffff;
}
</style>