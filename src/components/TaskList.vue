<template>
<div class="view-container">
    <template v-if="tasks.length > 0">
        <div v-for="(task, index) in filteredTasks" :key="index" class="task" :class="{ 'active': showDescription === index, 'task-incomplete': !task.isCompleted }" @click="toggleCard(index)">
            <div class="icons">
                <h3>{{ task.title }}</h3>
                <div class="icons-list">
                    <i class="material-icons" @click.stop="deleteTask(index)">delete</i>
                    <i class="material-icons" @click.stop="editTask(index)">edit</i>
                    <i class="material-icons check-icon" @click.stop="toggleCompletion(index)" :class="{ active: task.isCompleted }">check</i>
                </div>
            </div>
            <div v-if="showDescription === index">{{ task.details }}</div>
        </div>
    </template>
    <p v-else class="paragraph">No projects</p>
</div>
</template>

    
<script>
export default {
    name: "TaskList",
    props: {
        tasks: Array,
        showCompleted: Boolean,
    },
    data() {
        return {
            showDescription: null,
        }
    },
    computed: {
        filteredTasks() {
            if (this.showCompleted === null) {
                return this.tasks;
            }else{
                return this.tasks.filter(task => this.showCompleted ? task.isCompleted : !task.isCompleted);
            }
        }
    },
    methods: {
        toggleCard(index) {
            this.showDescription = this.showDescription === index ? null : index;
        },
        deleteTask(index) {
            this.$emit('delete-task', index);
        },
        editTask(index) {
            this.$emit('edit-task', index);
        },
        toggleCompletion(index) {
            this.$emit('toggle-completion', index);
        }
    }
}
</script>

<style scoped>
.view-container {
    width: 100%;
}

.task {
    border-left: 5px solid rgb(47, 187, 98);
    background-color: #ffffff;
    border-radius: 5px;
    transition: all 0.3s ease;
    cursor: pointer;
    padding: 10px 10px 10px 30px;
    margin: 20px 0;
}

.task-incomplete {
    border-left: 5px solid red;
}

.active {
    padding: 20px;
}

.icons {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.icons-list i {
    margin-right: 10px;
    color: #BBBBBB;
}

.icons-list .check-icon {
    font-weight: bold;
    color: #ff0000;
}

.icons-list .check-icon.active {
    color: rgb(47, 187, 98);
    margin: 0 10px 0 0;
    padding: 0;
}

.paragraph {
    font-size: 2rem;
    color: rgb(173, 173, 173);
    text-align: center;
}
</style>
