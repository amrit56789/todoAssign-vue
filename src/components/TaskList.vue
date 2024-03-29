<template>
<div class="view-container">
    <template v-if="tasks.length > 0">
        <div v-for="(task, index) in filteredTasks" :key="index" class="task" :class="{ 'active': showDescription === index, 'task-incomplete': !task.isCompleted }" @click="toggleCard(index)">
            <div class="icons">
                <div class="title">
                    <h3>{{ task.title }}</h3>
                </div>
                <div class="icons-list">
                    <i class="material-icons" @click.stop="deleteTask(index)">delete</i>
                    <i class="material-icons" @click.stop="editTask(index)">edit</i>
                    <i class="material-icons check-icon" @click.stop="toggleCompletion(index)" :class="{ active: task.isCompleted }">check</i>
                </div>
            </div>
            <div v-if="showDescription === index" class="details">{{ task.details }}</div>
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
            } else {
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
    padding: 10px;
    margin: 10px 0;
    box-sizing: border-box;
}

.icons-list i {
    margin-right: 10px;
    color: #BBBBBB;
    cursor: pointer;
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

.icons .title {
    width: 70%;
    box-sizing: border-box;
    overflow-wrap: break-word;
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

.details {
    width: 100%;
    box-sizing: border-box;
    overflow-wrap: break-word;
    margin-top: 10px;
}

.paragraph {
    font-size: 2rem;
    color: rgb(173, 173, 173);
    text-align: center;
}

@media (max-width: 768px) {
    .task {
        font-size: 0.9rem;
        padding: 10px 10px;
    }

    .icons {
        flex-direction: column;
        align-items: flex-start;
    }

    .icons h3 {
        margin-bottom: 10px;
    }

    .icons-list i {
        font-size: 20px;
    }
}

@media (max-width: 480px) {
    .task {
        margin: 10px 0;
    }

    .icons-list i {
        margin-right: 3px;
        font-size: 16px;
    }
    .icons{
        width: 100%;
        display: flex;
        flex-direction: row;
        align-items: center;
    }
    .icons .icons-list{
        width: 80%;
        display: flex;
        flex-direction: row;
        align-items: center;
        justify-content: flex-end;
    }
    .title {
        font-size: 12px;
        width: 100%;
        box-sizing: border-box;
        overflow-wrap: break-word;
    }

    .details {
        line-height: 22px;
        width: 100%;
        box-sizing: border-box;
        overflow-wrap: break-word;
        margin-top: 10px;
    }
}
</style>
