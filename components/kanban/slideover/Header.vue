<script setup lang="ts">
import dayjs from 'dayjs';
import { useDealSlideStore } from '~/store/deal-slide.store';

const store = useDealSlideStore();
</script>
<template>
  <div class="border-border bg-black/20 rounded p-3">
    <div class="uppercase bold text-xl mb-4">About deal</div>
    <KanbanSlideoverLabel labale-text="Deal title"
      ><h2>{{ store.card?.name }}</h2></KanbanSlideoverLabel
    >
    <KanbanSlideoverLabel labale-text="Price">{{ convertCurrency(store.card?.price || 0) }}</KanbanSlideoverLabel>
    <KanbanSlideoverLabel labale-text="Status">
      <UiBadge variant="outline">{{ store.card?.status }}</UiBadge>
    </KanbanSlideoverLabel>
    <KanbanSlideoverLabel labale-text="Customer">{{ store.card?.companyName }}</KanbanSlideoverLabel>
    <KanbanSlideoverLabel labale-text="Date">{{ dayjs(store.card?.$createdAt).format('DD MMMM YYYY') }}</KanbanSlideoverLabel>
  </div>
</template>
