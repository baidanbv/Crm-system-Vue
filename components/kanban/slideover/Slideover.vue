<script setup lang="ts">
import { useDealSlideStore } from '~/store/deal-slide.store';

const store = useDealSlideStore();

const isLocalOpen = computed({
  get: () => store.isOpen,
  set: (value) => (store.isOpen = value)
});
</script>

<template>
  <div>
    <USlideover v-model="isLocalOpen" :style="{backgroundColor: 'rgba(32, 41, 55,.7)' }">
      <UCard class="flex flex-col flex-1 overflow-y-auto"  :style="{ backgroundColor: 'var(--background)' }" :ui="{ body: { base: 'flex-1' }, ring: '', divide: 'divide-y divide-border'}">
          <template #header>
            <KanbanSlideoverHeader />
          </template>
          <KanbanSlideoverComments/>
      </UCard>
    </USlideover>
  </div>
</template>


<style >
 div.bg-gray-200\/75 {
  background-color: initial;
}
</style>