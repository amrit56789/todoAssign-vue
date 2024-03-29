<template>
    <div class="todo-card">
        <div class="todo-field">
            <label for="title">TITLE</label>
            <input type="text" id="title" v-model="task.title" />
            <p v-if="errors.title" class="error">{{ errors.title }}</p>
        </div>
        <div class="todo-field">
            <label for="text-area">DETAILS</label>
            <textarea id="text-area" rows="8" v-model="task.details"></textarea>
            <p v-if="errors.details" class="error">{{ errors.details }}</p>
        </div>
        <button class="btn" @click="submitTask" v-if="typeof task.index !== 'undefined'">Update Project</button>
        <button class="btn" @click="submitTask" v-else>Add Project</button>
    </div>
    </template>
    
    <script>
    export default {
        name: "addEdit",
        data() {
            return {
                task: {
                    title: "",
                    details: ""
                },
                editTasks: null,
                errors: {} // Added for form validation
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
                        this.errors = {}; 
                    }
                },
            },
        },
        methods: {
            submitTask() {
                this.errors = this.validateTask();
                if (Object.keys(this.errors).length > 0) return;
    
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
            validateTask() {
                let errors = {};
                if (!this.task.title.trim()) errors.title = "Title is required.";
                if (!this.task.details.trim()) errors.details = "Details are required.";
                return errors;
            }
        },
    }
    </script>

<style scoped>
.todo-card {
    border-radius: 10px;
    width: 90%;
    max-width: 500px;
    background-color: #FFFFFF;
    margin: 20px auto;
    padding: 20px;
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

.todo-field input,
.todo-field textarea {
    width: 95%;
    padding: 10px;
    border: 1px solid #F2F2F2;
}

.btn {
    width: 100%;
    padding: 10px;
    border-radius: 6px;
    color: #ffffff;
    font-weight: 600;
    background-color: #00B587;
    cursor: pointer;
    margin-top: 20px;
}

@media (max-width: 768px) {
    .todo-card {
        width: 95%;
    }

    .todo-field label {
        margin: 10px 0;
    }

    .btn {
        padding: 10px;
    }
}

@media (max-width: 480px) {
    .todo-card {
        width: 80%;
    }
    .todo-field label {
        font-size: 0.9rem;
    }
    
.todo-field input,
.todo-field textarea {
    width: 95%;
    padding: 10px;
    border: 1px solid #F2F2F2;
}
.btn {
    margin-top: 20px;
        padding: 10px;
    }
}
</style>
