<template>
  <div>
    <nav class="blue-grey lighten-1" role="navigation">
      <div class="nav-wrapper container"><i class="material-icons logo">list_alt</i></a></div>
    </nav>
    <div class="section no-pad-bot" id="index-banner">
      <div class="container">
        <h3 class="header center teal-text">Another To-Do App</h3>
        <form @submit.prevent="addTask">
          <div class="row center">
            <h6 class="header col s12 light">What needs to be done?</h6>
          </div>
          <input v-model="newTaskName" type="text" placeholder="Type a task">
        <div class="row center">
          <button class="btn-small waves-effect waves-light teal" type="submit">Submit <i class="material-icons right">send</i></button>
        </div>
        </form>
      </div>
    </div>
    <div class="container">
      <div class="section">
        <div class="row">
          <div class="col s12 m6">
            <div class="card-panel blue-grey">
              <span class="white-text">
                <center><i class="material-icons">alarm</i></center>
                <h5 class="center">Get these things done! </h5>
                <tasks-list
                  :task-list="tasksPending"
                  @completar="toggleTasks"/>
              </span>
            </div>
          </div>
          <div class="col s12 m6">
            <div class="icon-block">
              <div class="card-panel blue-grey">
                <span class="white-text">
                  <center><i class="material-icons">done_outline</i></center>
                  <h5 class="center">Hooray! </h5>
                  <tasks-list
                    :task-list="tasksComplete"
                    @completar="toggleTasks"/>
                </span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TasksList from './components/TasksList'
import Firebase from 'firebase'

let config = {
    apiKey: "...",
    authDomain: "...",
    databaseURL: "...",
    projectId: "...",
    storageBucket: "...",
    messagingSenderId: "..."
  };

let app = Firebase.initializeApp(config)
let db = app.database()
let taskRef = db.ref('tasks')

export default {
  components: {
    TasksList
  },
  firebase: {
    tasks: taskRef
  },
  data () {
    return {
      newTaskName: '',
      tasks: [
        {
          name: 'Tarea 1',
          done: false
        },
        {
          name: 'Tarea 2',
          done: false
        },
        {
          name: 'Tarea 3',
          done: false
        }
      ]
    }
  },
  methods: {
    toggleTasks (task) {
      task.done = !task.done
    },
    addTask () {
      if (!this.newTaskName) return
      taskRef.push({ name: this.newTaskName, done: false })
      this.newTaskName = ''
    }

  },
  computed: {
    tasksPending () {
      return this.tasks.filter(task => !task.done)
    },
    tasksComplete () {
      return this.tasks.filter(task => task.done)
    }
  }
}
</script>

<style>
  .card-panel { min-height: 450px;}
  .material-icons.logo { font-size: 50px; }
  .material-icons { font-size: 40px; }
</style>
