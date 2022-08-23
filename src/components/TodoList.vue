<template>
  <div class="container">
    <h2 class="text-center mt-5">To-Do List App</h2>

    <!-- input -->
    <div class="d-flex">
      <input
        v-model="task"
        type="text"
        placeholder="Enter task"
        class="form-control"
      />
      <button @click="submitTask" class="btn btn-primary rounded-1">
        Submit
      </button>
    </div>

    <!-- task table -->
    <table class="table table-bordered table-hover mt-5">
      <thead>
        <tr class="table-warning">
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">Edit</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{ Finished: task.status === 'Finished' }">
              {{ task.name }}
            </span>
          </td>
          <td>
            <span
              @click="changeStatus(index)"
              class="pointer"
              :class="{
                'text-danger': task.status === 'To-Do',
                'text-warning': task.status === 'In-progress',
                'text-secondary': task.status === 'Finished',
              }"
            >
              {{ task.status }}
            </span>
          </td>
          <td class="text-center" @click="editTask(index)">
            <span class="fa fa-pen"></span>
          </td>
          <td class="text-center" @click="deleteTask(index)">
            <span class="fa fa-trash"></span>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },

  data() {
    return {
      task: "",
      editedTask: null,
      statuses: ["To-Do", "In-progress", "Finished"],

      tasks: [
        {
          name: "learn Vue",
          status: "To-Do",
        },
        {
          name: "Keep going!",
          status: "In-progress",
        },
        {
          name: "eat lunch",
          status: "Finished",
        },
      ],
    };
  },

  methods: {
    submitTask() {
      if (this.task.length === 0) return;

      if (this.editedTask === null) {
        this.tasks.push({
          name: this.task,
          status: "To-Do",
        });
      } else {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task = "";
    },

    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },

    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
  },
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.Finished {
  text-decoration: line-through;
}
</style>
