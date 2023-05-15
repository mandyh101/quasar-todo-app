<template>
  <q-page class="q-pa-lg bg-grey-1 column">
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
            v-for="task in tasks"
            :key="task.title"
            v-ripple
            @click="toggleTask(task)"
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
          </q-item>
        </q-list>
      </div>

      <div class="q-px-sm q-mt-sm">
        Your selection is: <strong>{{}}</strong>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface Task {
  title: string;
  done: boolean;
}

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

const toggleTask = (task: Task) => {
  task.done = !task.done;
};
</script>
