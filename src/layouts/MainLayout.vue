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
      <div class="q-px-lg q-pt-xl q-md-md q-gutter-sm">
        <div class="text-h3">Things to do</div>
        <div class="text-sibtitle1">{{ todaysDate }}</div>
      </div>
      <q-img
        class="header-image absolute-top"
        src="../assets/rough-horn-2146181_1920.jpg"
        alt="background image of view from the top of a snow-capped mountain range"
      />
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list>
        <q-item-label header> Essential Links </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
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
