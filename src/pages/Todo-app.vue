<template>
  <h1 style="margin-left:40%">To-Do App</h1>
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
        <!-- Task Items -->
        <v-list-item
          v-for="(task, index) in tasks"
          :key="task.id"
        >
          <v-list-item-action class="d-flex align-center justify-space-between ">
            <div class="d-inline-flex">
              <v-checkbox
                v-model="task.done"
                class="d-flex align-lg-center"
                color="primary"
                :ripple="false"
              />
              <v-list-item-content class="d-flex align-center">
                <v-list-item-title :class="{ 'text-decoration-line-through': task.done }">{{ task.text }}</v-list-item-title>
              </v-list-item-content>
            </div>
            <v-list-item-action>
              <v-btn
                class="d-flex align-center justify-md-end "
                color="red"
                icon
                @click="deleteTask(index)"
              >
                <v-icon size="small">mdi-delete</v-icon>
              </v-btn>
            </v-list-item-action>
          </v-list-item-action>
        </v-list-item>

        <!-- Empty State Message -->
        <v-list-item v-if="tasks.length === 0">
          <v-list-item-title class="text-grey">
            No tasks found.
          </v-list-item-title>
        </v-list-item>
      </v-list>

    </v-card>
  </v-container>
</template>

<script setup lang="ts">
  import { onMounted, ref, watch } from 'vue'

  interface ITodo {
    id: number
    text: string
    done: boolean
  }

  const newTask = ref('')
  const tasks = ref<ITodo[]>([])

  const STORAGE_KEY = 'todo-list'

  onMounted(() => {
    const stored = localStorage.getItem(STORAGE_KEY)
    if (stored) {
      tasks.value = JSON.parse(stored)
    }
  })

  watch(
    tasks,
    newTasks => {
      localStorage.setItem(STORAGE_KEY, JSON.stringify(newTasks))
    },
    { deep: true },
  )
  const addTask = () => {
    const text = newTask.value.trim()
    if (text) {
      tasks.value.push({ id: Date.now(), text, done: false })
      newTask.value = ''
    }
  }

  const deleteTask = (index: number) => {
    tasks.value.splice(index, 1)
  }
</script>
