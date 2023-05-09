<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
      </q-toolbar>
      <div class="q-px-lg q-pt-md q-mb-md q-gutter-sm">
        <div class="text-h3">Things to do</div>
        <div class="text-sibtitle1">{{ todaysDate }}</div>
      </div>
      <q-img
        class="header-image absolute-top"
        src="../assets/rough-horn-2146181_1920.jpg"
        alt="background image of view from the top of a snow-capped mountain range"
      />
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="300"
      :breakpoint="600"
      overlay
      bordered
      :class="$q.dark.isActive ? 'bg-grey-9' : 'bg-grey-3'"
    >
      <div>
        <div
          class="text-weight-bold text-h3 q-px-md q-mt-xl q-pt-lg text-primary"
        >
          Hello Mandy!
        </div>
      </div>

      <q-list padding class="menu-list">
        <q-item to="/" clickable v-ripple>
          <q-item-section avatar>
            <q-icon name="task" />
          </q-item-section>

          <q-item-section> All todos </q-item-section>
        </q-item>
        <q-item to="/about" clickable v-ripple>
          <q-item-section avatar>
            <q-icon name="information" />
          </q-item-section>

          <q-item-section> About </q-item-section>
        </q-item>
      </q-list>
    </q-drawer>

    <q-page-container>
      <!-- keep alive will preserve changes made on a page if you switch pages -->
      <keep-alive>
        <router-view />
      </keep-alive>
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import { date } from 'quasar';

const todaysDate = computed(() => {
  let timeStamp = Date.now();
  return date.formatDate(timeStamp, 'dddd D MMMM');
});

const leftDrawerOpen = ref(false);

function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value;
}
</script>

<style lang="scss">
.header-image {
  // image should fit to header. z-index sends it behind otehr elements
  height: 100%;
  z-index: -1;
  opacity: 0.5;
  filter: grayscale(100%);
}
</style>
