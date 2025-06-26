<template>
  <h1 style="margin-left:45% ">To-Do App</h1>
  <v-container class="pa-4" style="max-width: 600px">
    <v-card>
      <v-card-title>
        <v-text-field
          v-model="newTask"
          append-inner-icon="mdi-plus"
          label="New Task"
          @click:append-inner="addTask"
          @keyup.enter="addTask"
        />
      </v-card-title>

      <v-divider />

      <v-list>
        <v-list-item
          v-for="(task,index) in tasks"
          :key="index"
          :class="{'text-decoration-line-through' :task.done}"
        >
          <v-list-item-action>
            <v-checkbox
              v-model="task.done"
              color="primary"
              :ripple="false"
            />
            <v-list-item-content>
              <v-list-item-title> {{ task.text }}</v-list-item-title>
            </v-list-item-content>
            <v-list-item-actions>

              <v-btn
                color="red"
                icon
                @click="deleteTask(index)"
              >
                <v-icon size="small">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-actions>

          </v-list-item-action>
          <v-list-item v-if="tasks.length === 0">
            <v-list-item-title class="text.grey">
              No tasks found.
            </v-list-item-title>
          </v-list-item>
        </v-list-item></v-list>
    </v-card>
  </v-container>
</template>

<script setup lang="ts">
  import { ref } from 'vue'

  interface ITodo {
    id: number
    text: string
    done: boolean
  }

  const newTask = ref('')
  const tasks = ref<ITodo[]>([])

  const addTask = () => {
    const text = newTask.value.trim()
    if (text) {
      tasks.value.push({ text, done: false })
      newTask.value = ''
    }
  }

  const deleteTask = index => {
    tasks.value.splice(index, 1)
  }
</script>
