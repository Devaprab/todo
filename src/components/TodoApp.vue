<template>
    <div class="container">
        <h2 class="text-center mt-5">My Todo App</h2>

        <!--input-->
        <div class="d-flex">
            <input v-model="task" type="text" placeholder="Enter Task" class="form-control">
            <button @click="submitTask" class="btn btn-warning rounded-0">SUBMIT</button>
        </div>

        <!--Task Table-->
        <table class="table table-bordered mt-5">
  <thead>
    <tr>
      <th scope="col">Task</th>
      <th scope="col">Status</th>
      <th scope="col" class="text-center">Edit</th>
      <th scope="col" class="text-center">Delete</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task,index) in tasks" :key="index">
      <td>
        <span :class="{'finished': task.status === 'finished'}">{{ task.name }}</span>
      </td>
      <td style="width: 120px">
        <span v-if="task.status !== 'finished'" @click="changeStatus(index)" class="pointer"  
          :class="{ 'text-danger' : task.status === 'to-do',
          'text-warning' : task.status === 'in-progress',
          'text-success' : task.status === 'finished'
        }"
        >
          {{ firstCharUpper(task.status) }}
        </span>
        <span v-else class="text-success" disabled>
             {{ firstCharUpper(task.status) }}
        </span>
      </td>
      <td>
        <div class="text-center" @click="editTask(index)">
            <span class="fa fa-pen"></span>
        </div>
      </td>
      <td>
        <div class="text-center" @click="deleteTask(index)">
            <span class="fa fa-trash"></span>
        </div>
      </td>
    </tr>
  </tbody>
</table>
 <!-- Finished Task Table -->
 <!-- <h2 class="text-center mt-5">Finished Tasks</h2>
    <table class="table table-bordered">
      <thead>
        <tr>
          <th scope="col">Task</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in finishedTasks" :key="index">
          <td>
            <span :class="{'finished': task.status === 'finished'}">{{ task.name }}</span>
          </td>
        </tr>
      </tbody>
    </table> -->

    </div>
</template>

<script>
export default {
  props: {
    msg: String
  },
  data() {
    return {
        task: '',
        editedTask: null,
        availableStatuses: ['to-do','in-progress','finished'],
        tasks:[
            // {
            //     name:'Do HomeWork',
            //     status: 'to-do',
            // },
            // {
            //     name:'Eat Fruits.',
            //     status: 'in-progress',
            // }
        ]
    }
  },
  // computed: {
  //   finishedTasks() {
  //     return this.tasks.filter(task => task.status === 'finished');
  //   }
  // },
  methods: {
    submitTask(){
        if(this.task.length === 0) return;

        if(this.editedTask === null){
             this.tasks.push({
                name: this.task,
                status: 'to-do'
            });
        }else {
            this.tasks[this.editedTask].name = this.task;
            this.editedTask = null;
        }

        this.task = '';
    },
    deleteTask(index) {
        this.tasks.splice(index, 1);
    },
    editTask(index) {
        this.task = this.tasks[index].name;
        this.editedTask = index;
    },
    changeStatus(index) {
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2)
      {
        // this.deleteTask(index);
         return;
      }
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
    firstCharUpper(str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
};
</script>

<style scoped>

.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>