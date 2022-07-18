<template>
    <AddTask v-show="showAddTask" @add-task="addTask" />
    <Tasks
        @toggle-reminder="toggleReminder"
        @delete-task= "deleteTask"
        :tasks="tasks"
    />
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'
export default {
    name: 'Home',
    props:{
        showAddTask: Boolean,
    },
    components: {
        Tasks,
        AddTask
    },
    data() {
        return {
            // function return object tasks
            // create life cycle method
            tasks: [],
            //showAddTask: false
        }
    },
    methods: {
        async addTask(task) {
            const res = await fetch('api/tasks', {
            method: 'POST',
            headers: {
                'content-type': 'application/json',
            },
            body: JSON.stringify(task)
        })

        const data = await res.json();

        this.tasks = [...this.tasks, data];
        },
        async deleteTask(id) {
            // filter every task but that id given, meaning we are removing that task with that id
            if (confirm('Are you sure?')) {
                // make request
                const res = await fetch(`api/tasks/${id}`, {
                    method: "DELETE",
                })

                // check if successful, then delete the task from list, if not, alert user
                res.status === 200 ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error deleting task')
            }
        },
        async fetchTask(id) {
            const res = await fetch(`api/tasks/${id}`); // fetch data

            const data = await res.json(); // convert to json

            return data; //return data in json format
        },
        async toggleReminder(id) {
            // create request
            const taskToToggle = await this.fetchTask(id);
            const updTask = {...taskToToggle, reminder:!taskToToggle.reminder}

            const res = await fetch(`api/tasks/${id}`, {
                method: 'PUT',
                headers: {
                'Content-type': 'application/json'
                },
                body: JSON.stringify(updTask),
            })

            const data = await res.json();
            // map each task and return a new list of task.
            // Checking if we are on that task id, then we switch the reminder bool to its opposite
            this.tasks = this.tasks.map((task) => task.id === id ? {...task, reminder: data.reminder} : task)
        },
        async fetchTasks() {
            const res = await fetch('api/tasks'); // fetch data
            const data = await res.json(); // convert to json
            return data; //return data in json format
        }
    },
    async created() {
        this.tasks = await this.fetchTasks();
    }
}
</script>
