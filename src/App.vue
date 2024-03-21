<template>
<div id="app">
    <div class="container">
        <div class="content-wrapper">
            <div class="tabs">
                <button @click="setActiveTab('project')" :class="{ active: activeTab === 'project' }">Project</button>
                <button @click="setActiveTab('newProject')" :class="{ active: activeTab === 'newProject' }">New Project</button>
            </div>

            <div class="tab-content" v-if="activeTab === 'project'">
                <div class="sub-tabs">
                    <button @click="setActiveProjectView('viewAll')" :class="{ active: activeProjectView === 'viewAll' }">VIEW ALL</button>
                    <button @click="setActiveProjectView('completed')" :class="{ active: activeProjectView === 'completed' }">COMPLETED</button>
                    <button @click="setActiveProjectView('ongoing')" :class="{ active: activeProjectView === 'ongoing' }">ONGOING</button>
                </div>
                <div class="project-list">
                    <div class="project-list">
                        <TaskList v-if="activeProjectView === 'viewAll'" :tasks="tasks" :showCompleted=null @delete-task="deleteTask" @edit-task="editTask" @toggle-completion="toggleTaskCompletion" />
                        <TaskList v-if="activeProjectView === 'completed'" :tasks="tasks" :showCompleted="true" @delete-task="deleteTask" @edit-task="editTask" @toggle-completion="toggleTaskCompletion" />
                        <TaskList v-if="activeProjectView === 'ongoing'" :tasks="tasks" :showCompleted="false" @delete-task="deleteTask" @edit-task="editTask" @toggle-completion="toggleTaskCompletion" />

                    </div>
                </div>
            </div>
        </div>
        <div class="new-project">
            <NewProject v-if="activeTab === 'newProject'" :taskToEdit="taskToEdit" @add-task="addTask" @update-task="updateTask" />
        </div>
    </div>
</div>
</template>

<script>
import NewProject from './components/NewProject.vue'
import TaskList from './components/TaskList.vue'

export default {
    name: 'App',
    components: {
        NewProject,
        TaskList
    },
    data() {
        return {
            activeTab: 'project',
            activeProjectView: 'viewAll',
            tasks: [],
            taskToEdit: null,
        }
    },
    methods: {
        setActiveTab(tab) {
            this.activeTab = tab;
            if (tab === 'project') {
                this.activeProjectView = 'viewAll';
            }
        },
        setActiveProjectView(view) {
            this.activeProjectView = view;
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
    width: 50%;
    padding: 30px 0;
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
    margin: 10px 40px;
}

.new-project {
    width: 100%;
}

.project-list {
    width: 100%;
}
</style>
