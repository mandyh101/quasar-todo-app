<template>
  <q-page class="q-pa-md bg-grey-2 column">
    <div class="q-pa-sm">
      <q-input
        v-model="newTask"
        label="Add task"
        label-color="primary"
        color="grey-5"
        outlined
        @keyup.enter="addTask(newTask)"
      >
        <template v-slot:append>
          <q-btn
            color="primary"
            round
            dense
            flat
            icon="add"
            @click="addTask(newTask)"
          />
        </template>
      </q-input>
    </div>
    <div class="q-pa-xs q-mt-md">
      <div class="text-h5 q-mb-md">Your tasks</div>
      <div class="q-gutter-sm">
        <q-list separator class="bg-grey-2">
          <!--
        Rendering a <label> tag (notice tag="label")
        so QCheckboxes will respond to clicks on QItems to
        change Toggle state.
      -->
          <q-item
            v-for="(task, index) in tasks"
            :key="index"
            v-ripple
            @click="toggleTask(task)"
            :class="{ 'done bg-green-1': task.done }"
            clickable
          >
            <q-item-section avatar>
              <q-checkbox v-model="task.done" color="primary" />
            </q-item-section>
            <q-item-section>
              <q-item-label>{{ task.title }}</q-item-label>
            </q-item-section>
            <q-item-section v-if="task.done" side>
              <!-- TODO style button -->
              <!-- TODO add a delete all button or swpie effect -->
              <q-btn
                flat
                round
                dense
                color="primary"
                class="text-red-5"
                @click.stop="confirmDelete(index)"
                icon="delete"
              />
            </q-item-section>
          </q-item>
        </q-list>
        <div v-if="!tasks.length" class="no-tasks absolute-center">
          <q-icon name="check" size="100px" color="primary" />
          <div class="text-h5 text-primary text-center">No tasks!</div>
        </div>
      </div>
      <!-- TODO style dialog -->
      <q-dialog v-model="confirm" persistent>
        <q-card>
          <q-card-section class="row items-center">
            <q-avatar icon="delete" color="primary" text-color="white" />
            <span class="q-ml-sm">Delete this task?</span>
          </q-card-section>

          <q-card-actions align="right">
            <q-btn flat label="Cancel" color="grey-5" v-close-popup />
            <q-btn flat label="Delete it!" color="primary" v-close-popup />
          </q-card-actions>
        </q-card>
      </q-dialog>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { useQuasar } from 'quasar';

const $q = useQuasar();

interface Task {
  title: string;
  done: boolean;
}

const confirm = ref(false);
const newTask = ref('');

const tasks = ref<Task[]>([]);

//used to show a task as complete or todo
const toggleTask = (task: Task) => {
  task.done = !task.done;
};

// Creates a new task using the string value passed from the add task input
const addTask = (taskTitle: string) => {
  const newTaskObj: Task = {
    title: taskTitle,
    done: false,
  };
  tasks.value.push(newTaskObj);

  // Save the new tasks locally
  try {
    $q.localStorage.set('tasks', tasks.value);
    console.log(tasks.value);
  } catch (e) {
    console.log(e);
  }

  // Clear input
  newTask.value = '';
};

// Confirm user wants to delete this task and delete on ok. Else do nothing.
const confirmDelete = (index: number) => {
  $q.dialog({
    title: 'Confirm',
    message: 'Delete this task?',
    cancel: true,
    persistent: true,
  }).onOk(() => {
    tasks.value.splice(index, 1); //delete from the frontend tasks array
    $q.localStorage.set('tasks', tasks.value); //set the data in the tasks key in local storage to match the updated tasks array.

    showNotif();
  });
};

// Shows a toast when a task is successfully deleted
const showNotif = () => {
  $q.notify({
    message: 'Task deleted',
    color: 'primary',
  });
};

onMounted(() => {
  // $q.localStorage.remove('tasks');
  const storedTasksJson = $q.localStorage.getItem('tasks');
  if (storedTasksJson) {
    const storedTasks: Task[] = storedTasksJson as Task[];
    tasks.value = storedTasks;
    console.log(tasks.value);
  }
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks {
  opacity: 0.5;
}
</style>
