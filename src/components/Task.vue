<template>
<!-- using ternary to check if reminder is true or false -->
    <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder' : '', 'task']">
        <h3> {{ task.text }}
            <i @click="onDelete(task.id)" class="fas fa-times"></i>
        </h3>
        <p>{{ "Due on: " + task.date + " at " + task.time}}</p>
    </div>
</template>

<script>
    export default {
        name: "Task",
        props: {
            task: Object
        },
        methods: {
            // we can create a method, or $emit (check dbclick example)
            onDelete(id) {
                // To be able to pass the delete to App
                this.$emit('delete-task', id)
            },
        },
    }
</script>

<style scope>

.fas {
    color: red;
}

.task {
  background: #dddbdb;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>