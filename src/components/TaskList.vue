<template>
  <div>
    <input type="text" class="task-input" 
    placeholder="What do you need to do?" v-model="newTask" @keyup.enter="addTask">
    <div v-for="(task, index) in tasks" :key="task.id" class="task">
      <div class="task__title">
        {{ task.title }}
      </div>

      <div class="task__actions" >
        <div class="remove-task" @click="removeTask(index)">
          &times;
        </div>
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  name: 'task-list',
  data() {
    return {
      newTask: '',
      idForTask: 4,
      tasks: [
        {
          id: 1,
          title: 'Finish learning Vue',
          completed: false,
        },
        {
          id: 2,
          title: 'Learn React Native',
          completed: false,
        },
        {
          id: 3,
          title: 'Learn Swift',
          completed: false,
        },
      ],
    };
  },
  methods: {
    addTask() {

      if (this.newTask.trim().length === 0) {
        return
      }
      this.tasks.push({
        id: this.idForTask,
        title: this.newTask,
        completed: false,
      })

      this.newTask = '';
      this.idForTask++;
    },
    removeTask(index) {
      this.tasks.splice(index, 1)
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss'>
  .task-input {
    width: 100%;
    padding: 10px;
    font-size: 18px;
    margin-bottom: 15px;
    &:focus {
      outline: 0;
    }
  }

  .task {
      margin-bottom: 10px;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    .remove-task {
      cursor: pointer;
      margin-left: 14px;

      &:hover {
        color: red;
      }
    }
</style>
