<template>
  <div>
    <div class="filters">
      <div class="buttons">
        <button :class="{active: filter == 'all'}" @click="filter = 'all'">
          All
        </button>
        <button :class="{active: filter == 'active'}" @click="filter = 'active'">
          Active
        </button>
        <button :class="{active: filter == 'completed'}" @click="filter = 'completed'">
          Completed
        </button>
      </div>
      <div class="clear-completed">
        <transition name="fade">
          <button v-if="showClearCompletedButton" @click="clearCompleted">
            Clear completed
          </button>
        </transition>
      </div>
    </div>
    <input type="text" class="task-input" 
    placeholder="What do you need to do?" v-model="newTask" @keyup.enter="addTask">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
      <div v-for="(task, index) in filteredTasks" :key="task.id" class="task">
        <div class="task-title" >
          <input type="checkbox" v-model="task.completed">
          <div class="task-title__label" v-if="!task.editing" @click="editTask(task)" :class="{ completed : task.completed}">
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
    </transition-group>
    <div class="bottom">
      <div>
        <label for="">
          <input type="checkbox" @change="checkAllTasks" :checked='!noneRemaining'> Check All
        </label>
      </div>
      <div>{{remaining}} items left</div>
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
      filter: 'all',
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
  computed: {
    remaining() {
      return this.tasks.filter(task => !task.completed).length
    },
    noneRemaining() {
      return this.remaining != 0;
    },
    filteredTasks() {
      if (this.filter === 'all') {
        return this.tasks
      } else if (this.filter === 'active') {
        return this.tasks.filter(task => !task.completed)
      } else if (this.filter === 'completed') {
        return this.tasks.filter(task => task.completed)
      }

      return this.tasks
    },
    showClearCompletedButton() {
      return this.tasks.filter(task => task.completed).length > 0;
    }
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
      if (newTask.title.trim().length === 0) {
        task.title = this.beforeEditCache;
      }
      
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
    checkAllTasks() {
      this.tasks.forEach(task => task.completed = event.target.checked)
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(task => !task.completed)
    }
  },
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='scss'>
  @import url('https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.0/animate.css');
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

      .task-title {
        display: flex;
        justify-content: space-evenly;
      }
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

  .completed {
    text-decoration: line-through;
    color: red;
  }

  .bottom,
  .filters {
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-top: 1px solid lightgrey;
    padding-top: 14px;
    margin-bottom: 12px;

  }

  button {
    background-color: white;
    appearance: none;

    &:hover {
      background: lightgreen;
    }

    &:focus {
      outline: none;
    }
  }

  .active {
    background: lightgreen;

  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity .4s;
  }

  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }
</style>
