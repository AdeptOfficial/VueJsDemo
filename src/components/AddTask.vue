<template>
    <form @submit="onSubmit" class="add-form">
        <div class="form-control">
            <label>Task</label>
            <input type="text" v-model="text" name="text" placeholder="Add Task"/>
        </div>

        <div class="form-control">
            <label>Time</label>
            <input type="time" v-model="time" name="time" placeholder="Add Time"/>
        </div>

        <div class="form-control">
          <label>Date</label>
          <input type="date" v-model="date" name="date" placeholder="Add Date"/>
        </div>

        <div class="form-control form-control-check">
            <label>Set Reminder</label>
            <input type="checkbox" v-model="reminder" name="reminder" />
        </div>

         <input type="submit" value="Save Task" class="btn btn-block" />

    </form>
</template>

<script>
    export default {
        name: 'AddTask',
        data() {
            return {
                // connected with v-model
                text: '',
                time: '',
                date: '',
                reminder: false,
            }
        },
        methods: {
            onSubmit(e) {
              // error checking
                e.preventDefault();

                if (!this.text) {
                    alert('Please add a task');
                    return;
                }

                if (!this.time) {
                    alert('Please Choose a time');
                    return;
                }

                if (!this.date) {
                    alert('Please choose a date');
                    return;
                }


                // create new task
                const newTask = {
                    // id: Math.floor(Math.random() * 100000), // set random id for example sake
                    text: this.text,
                    time: this.time,
                    date: this.date,
                    //day: this.day,
                    reminder: this.reminder
                }

                console.log("New Task: " + newTask.text + "\nDue: " + newTask.date + " at " + newTask.time + "\nNew Task created!");
                this.$emit('add-task', newTask);

                // clear form for new usage
                this.text = ''
                this.time = ''
                this.date = ''
                this.reminder = false

            }
        }
    }
</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>