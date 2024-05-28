<script setup lang="ts">
import type { ICard, IColumn } from '~/components/kanban/kanban.types';
import { generateGradients } from '~/components/kanban/generate-gradient';
import { useKanbanQuery } from '~/components/kanban/useKanbanQuery';
import dayjs from 'dayjs';
import { useMutation } from '@tanstack/vue-query';
import { COLLECTION_DEALS, DB_ID } from '~/app.constants';
import { useDealSlideStore } from '~/store/deal-slide.store';
useSeoMeta({
  title: 'Home | CRM System'
});

const dragCardRef = ref<ICard | null>(null);
const sourceColumnRef = ref<IColumn | null>(null);
const { data, isLoading, refetch } = useKanbanQuery();

type TypeMutationValues = {
  docId: string;
  status: string;
};

const { mutate } = useMutation({
  mutationKey: ['move card'],
  mutationFn: ({ docId, status }: TypeMutationValues) => DB.updateDocument(DB_ID, COLLECTION_DEALS, docId, { status }),
  onSuccess: () => {
    refetch();
  }
});

const handleDragStart = (card: ICard, column: IColumn) => {
  dragCardRef.value = card;
  sourceColumnRef.value = column;
};

const handleDragOver = (e: DragEvent) => {
  e.preventDefault();
};

const handleDrop = (targetColumn: IColumn) => {
  if (dragCardRef.value && sourceColumnRef.value) {
    mutate({ docId: dragCardRef.value.id, status: targetColumn.id });
  }
};

const store = useDealSlideStore();

</script>

<template>
  <div class="p-10">
    <h1 class="font-bold text-2xl mb-10 text-secondary-foreground">CRM System by Web Vibe</h1>
    <div>
      <div v-if="isLoading">Loading...</div>
      <div v-else>
        <div class="grid grid-cols-5 gap-6">
          <div v-for="(column, index) in data"  @dragover="handleDragOver" @drop="() => handleDrop(column)" :key="column.id" class="min-h-screen">
            <div class="rounded bg-slate-700 py-1 px-5 mb-2 text-center" :style="generateGradients(index, data?.length)">{{ column.name }}</div>
            <div>
              <KanbanCreateDeal :refetch="refetch" :status="column.id" />
              <UiCard v-for="card in column.items" class="mb-5  " draggable="true" @dragstart="() => handleDragStart(card, column)">
                <UiCardHeader role="button" @click="store.set(card)">
                  <UiCardTitle>{{ card.name }}</UiCardTitle>
                  <UiCardDescription>{{ convertCurrency(card.price) }}</UiCardDescription>
                </UiCardHeader>
                <UiCardContent class="text-xs"
                  ><div>Company</div>
                  {{ card.companyName }}</UiCardContent
                >
                <UiCardFooter>{{ dayjs(card.$createdAt).format('DD-MMMM-YYYY') }}</UiCardFooter>
              </UiCard>
            </div>
          </div>
        </div>
        <KanbanSlideover/>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
