<template>
  <div>
    <input type="text" class="task-input" 
    placeholder="What do you need to do?" v-model="newTask" @keyup.enter="addTask">
    <div v-for="(task, index) in tasks" :key="task.id" class="task">
      <div class="task-title" >
        <div class="task-title__label" v-if="!task.editing" @click="editTask(task)">
          {{ task.title }}
        </div>
        <input v-else type="text" v-model="task.title" class="task-edit" @blur="doneEditing(task)" 
        @keyup.enter="doneEditing(task)" v-focus @keyup.esc="cancelEdit(task)"> 
      </div>

      <div class="task-actions" >
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
      beforeEditCache: '',
      tasks: [
        {
          id: 1,
          title: 'Finish learning Vue',
          completed: false,
          editing: false,
        },
        {
          id: 2,
          title: 'Learn React Native',
          completed: false,
          editing: false,
        },
        {
          id: 3,
          title: 'Learn Swift',
          completed: false,
          editing: false,
        },
      ],
    };
  },
  directives: {
    focus: {
      inserted(el) { el.focus(); },
    },
  },
  methods: {
    addTask() {
      if (this.newTask.trim().length === 0) {
        return;
      }
      this.tasks.push({
        id: this.idForTask,
        title: this.newTask,
        completed: false,
        editing: false,
      });

      this.newTask = '';
      this.idForTask += 1;
    },
    editTask(task) {
      const newTask = task;
      this.beforeEditCache = newTask.title;
      newTask.editing = true;
    },

    doneEditing(task) {
      const newTask = task;
      newTask.editing = false;
    },

    cancelEdit(task) {
      const newTask = task;
      task.title = this.beforeEditCache
      newTask.editing = false;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    },
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

  .task-title__label {
    padding: 10px;
    border: 1px solid white;
  }

  .task-edit {
    font-size: 20px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 10px;
    border: 10px;
    border: 1px solid #ccc;
    font-family: inherit;

    &:focus {
      outline: none;
    }
  }
</style>
