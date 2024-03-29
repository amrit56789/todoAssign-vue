<template>
<div id="app">
    <div class="container">
        <div class="content-wrapper">
            <div class="tabs">
                <button @click="setActiveTab('project')" :class="{ active: activeTab === 'project' }">Projects</button>
                <button @click="setActiveTab('newProject')" :class="{ active: activeTab === 'newProject' }">{{ this.taskToEdit!==null ? 'Update Project' : 'Add a New Project' }}</button>
            </div>
            <div class="tab-content" v-if="activeTab === 'project'">
                <div class="sub-tabs">
                    <button @click="setactiveProjectTab('viewAll')" :class="{ active: activeProjectTab === 'viewAll' }">VIEW ALL</button>
                    <button @click="setactiveProjectTab('completed')" :class="{ active: activeProjectTab === 'completed' }">COMPLETED</button>
                    <button @click="setactiveProjectTab('ongoing')" :class="{ active: activeProjectTab === 'ongoing' }">ONGOING</button>
                </div>
                <div class="project-list">
                    <div class="project-list">
                        <TaskList :tasks="tasks" :showCompleted="computedShowCompleted" @delete-task="deleteTask" @edit-task="editTask" @toggle-completion="toggleTaskCompletion" />
                    </div>
                </div>
            </div>
        </div>
        <div class="new-project">
            <addEdit v-if="activeTab === 'newProject'" :taskToEdit="taskToEdit" @add-task="addTask" @update-task="updateTask" />
        </div>
    </div>
</div>
</template>

<script>
import addEdit from './components/addEdit.vue'
import TaskList from './components/TaskList.vue'

export default {
    name: 'App',
    components: {
        addEdit,
        TaskList
    },
    data() {
        return {
            activeTab: 'project',
            activeProjectTab: 'viewAll',
            tasks: [],
            taskToEdit: null,
        }
    },
    computed: {
        computedShowCompleted() {
            if (this.activeProjectTab === 'viewAll') {
                return null;
            } else if (this.activeProjectTab === 'completed') {
                return true;
            } else if (this.activeProjectTab === 'ongoing') {
                return false;
            }
            return null;
        }
    },
    methods: {
        setActiveTab(tab) {
            this.activeTab = tab;
            if (tab === 'project') {
                this.activeProjectTab = 'viewAll';
            }
        },
        setactiveProjectTab(view) {
            this.activeProjectTab = view;
        },
        addTask(newTask) {
            this.tasks.push(newTask);
        },
        deleteTask(index) {
            this.tasks.splice(index, 1);
        },
        editTask(index) {
            this.taskToEdit = {
                ...this.tasks[index],
                index
            };
            this.setActiveTab('newProject');
        },
        toggleTaskCompletion(index) {
            this.tasks[index].isCompleted = !this.tasks[index].isCompleted;
        },
        updateTask(updatedTask) {
            if (updatedTask.index !== undefined) {
                this.tasks[updatedTask.index] = {
                    title: updatedTask.title,
                    details: updatedTask.details,
                    isCompleted: updatedTask.isCompleted
                };
                this.taskToEdit = null;
            }
        },
    }
}
</script>

<style>
#app {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
}

.container {
    margin-top: 20px;
    border-radius: 10px;
    background-color: #F2F2F2;
    width: 90%;
    padding: 30px 1%;
    max-width: 600px;
}

.content-wrapper {
    width: 100%;
    display: flex;
    flex-direction: column;
}

.tabs {
    margin: auto;
}

.tabs button {
    font-size: 1.5rem;
}

.sub-tabs {
    width: 100%;
}

.sub-tabs button {
    font-size: 0.8rem;
}

.tabs button,
.sub-tabs button {
    padding: 10px;
    margin-right: 10px;
    border: none;
    cursor: pointer;
    color: #a6a6a6;
    font-weight: bold;
    transition: background-color 0.3s;
}

.tabs button.active {
    border-bottom: 2px solid rgb(241, 183, 183);
}

.sub-tabs button.active {
    color: red;
}

.tab-content,
.sub-tabs {
    margin-top: 20px;
}

.tab-content {
    margin: 10px 10px;
}

.new-project {
    width: 100%;
}

.project-list {
    width: 100%;
}

@media (max-width: 768px) {

    .tabs button,
    .sub-tabs button {
        font-size: 1rem;
    }

    .tabs,
    .sub-tabs {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
    }

    .tabs button,
    .sub-tabs button {
        padding: 5px;
        margin: 5px;
    }

    @media (max-width: 480px) {

        .tabs button,
        .sub-tabs button {
            font-size: 0.8rem;
        }

        .container {
            padding: 30px 5%;
        }

        .content-wrapper,
        .new-project,
        .project-list {
            width: 100%;
        }
    }
}
</style>
