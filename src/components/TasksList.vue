<template>
  <div class="list">
    <table>
        <tbody>
          <tr v-for="(task, index) in taskList"
              :key="index">
            <td class="task-name">{{ task.name }}</td>
            <td class="task-action"><button class="waves-effect waves-light btn-small" v-if="!task.done" type="button" @click="toggleTasks(task)">Completar</button></td>
          </tr>
        </tbody>
      </table>
  </div>
</template>

<script>
export default {
  props: {
    taskList: {
      type: Array,
      required: true
    }
  },
  methods: {
    toggleTasks (task) {
      this.$emit('completar', task)
    }
  }
}
</script>

<style scoped>
  .task-name {
    width: 80%
  }
  .task-action {
    width: 20%;
  }
</style>
