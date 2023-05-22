<template>
  <q-page class="q-pa-lg bg-grey-1 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        placeholder="Add task"
        dense
        square
        class="col q-px-xs bg-white"
        @keyup.enter="addTask(newTask)"
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask(newTask)" />
        </template>
      </q-input>
    </div>
    <div class="text-h4">Your tasks</div>
    <div class="q-pa-md">
      <div class="q-gutter-sm">
        <q-list separator class="bg-white">
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
              <q-checkbox
                v-model="task.done"
                color="primary"
                class="no-pointer-events"
              />
            </q-item-section>
            <q-item-section>
              <q-item-label>{{ task.title }}</q-item-label>
            </q-item-section>
            <q-item-section v-if="task.done" side>
              <!-- TODO style button -->
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

      <div class="q-px-sm q-mt-sm">
        Your selection is: <strong>{{}}</strong>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { useQuasar } from 'quasar';

const $q = useQuasar();

interface Task {
  title: string;
  done: boolean;
}

const confirm = ref(<boolean>false);
let newTask = ref(<string>'');

const tasks = ref(<Task[]>[
  {
    title: 'Get bananas',
    done: false,
  },
  {
    title: 'Make cake with bananas',
    done: true,
  },
  {
    title: 'Eat banana cake',
    done: false,
  },
]);

//mark task as done or todo
const toggleTask = (task: Task) => {
  task.done = !task.done;
};

//confimr user wants to delete this task and delte on ok. Else do nothing.
const confirmDelete = (index: number) => {
  $q.dialog({
    title: 'Confirm',
    message: 'Delete this task?',
    cancel: true,
    persistent: true,
  }).onOk(() => {
    tasks.value.splice(index, 1);
    showNotif();
  });
};

//creates a new task using the string value passed from the add task input
const addTask = (taskTitle: string) => {
  tasks.value.push({
    title: taskTitle,
    done: false,
  });
  //clear input
  newTask.value = '';
};

//shows a toast when note is successfully deleted
const showNotif = () => {
  $q.notify({
    message: 'Task deleted',
    color: 'primary',
  });
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>
