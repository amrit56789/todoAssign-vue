<template>
<div class="todo-card">
    <div class="todo-field">
        <label for="title">TITLE</label>
        <input type="text" id="title" v-model="task.title" />
    </div>
    <div class="todo-field">
        <label for="text-area">DETAILS</label>
        <textarea type="textarea" id="text-area" rows="8" v-model="task.details"></textarea>
    </div>
    <button class="btn" @click="submitTask">Add project</button>
</div>
</template>

<script>
export default {
    name: "NewProject",
    data() {
        return {
            task: {
                title: "",
                details: ""
            },
            editTasks: null,
        }
    },
    props: {
        taskToEdit: {
            type: Object,
            default: () => ({}),
        },
    },
    watch: {
        taskToEdit: {
            immediate: true,
            handler(newValue) {
                if (newValue && Object.keys(newValue).length !== 0) {
                    this.task = {
                        ...newValue
                    };
                }
            },
        },
    },
    methods: {
        submitTask() {
            if (this.task.title.trim() === "" || this.task.details.trim() === "") return;

            if (this.task.index !== undefined) {
                this.$emit('update-task', {
                    title: this.task.title,
                    details: this.task.details,
                    isCompleted: this.task.isCompleted,
                    index: this.task.index,
                });
            } else {
                this.$emit('add-task', {
                    index: Date.now(),
                    title: this.task.title,
                    details: this.task.details,
                    isCompleted: false
                });
            }

            this.resetTask();
        },
        resetTask() {
            this.task = {
                title: "",
                details: ""
            };
        },
    },
}
</script>

<style scoped>
.todo-card {
    border-radius: 10px;
    width: 80%;
    background-color: #FFFFFF;
    margin: auto;
    padding: 20px;
    margin-top: 20px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.todo-field {
    display: flex;
    flex-direction: column;
    color: #cca0a0;
}

.todo-field label {
    margin: 10px 0;
    text-align: start;
    font-weight: 550;
    letter-spacing: 1px;
}

.todo-field input {
    margin-bottom: 20px;
    padding: 10px 8px;
    border: none;
    border-bottom: 1px solid #F2F2F2;
}

.todo-field textarea {
    border: 1px solid #F2F2F2;
    padding: 10px 8px;
}

.btn {
    margin-top: 30px;
    border: none;
    padding: 10px 30px;
    border-radius: 6px;
    color: #ffffff;
    font-weight: 600;
    background-color: #00B587;
    cursor: pointer;
}
</style>
