<script setup lang="ts">
import { useMutation } from '@tanstack/vue-query';
import { v4 as uuid } from 'uuid';
import { defineProps } from 'vue';
import { COLLECTION_DEALS, DB_ID } from '@/app.constants';
import type { IDeal } from '~/types/deals.types';

const isOpenForm = ref(false);

interface IDealFormState extends Pick<IDeal, 'name' | 'price'> {
  customer: {
    email: string;
    name: string;
  };
  status: string;
}

const props = defineProps({
  status: {
    type: String,
    default: ''
  },
  refetch: {
    type: Function
  }
});

const { handleSubmit, defineField, handleReset } = useForm<IDealFormState>({
  initialValues: {
    status: props.status
  }
});

const [name, nameAttrs] = defineField('name');
const [price, priceAttrs] = defineField('price');
const [customerEmail, customerEmailAttrs] = defineField('customer.email');
const [customerName, customerNameAttrs] = defineField('customer.name');

const { mutate, isPending } = useMutation({
  mutationKey: ['create a new deal'],
  mutationFn: (data: IDealFormState) => {
    data.price = parseInt(data.price as unknown as string, 10);
    if (isNaN(data.price)) {
      throw new Error('Price must be a valid integer');
    }
    return DB.createDocument(DB_ID, COLLECTION_DEALS, uuid(), data);
  },
  onSuccess: () => {
    props.refetch && props.refetch();
    handleReset();
	}
});

const onSubmit = handleSubmit((data) => {
  mutate(data);
});
</script>

<template>
  <div class="text-center mb-2">
    <button class="transition-all opacity-30 hover:opacity-100 hover:text-[#a252c8]" @click="isOpenForm = !isOpenForm">
      <Icon v-if="isOpenForm" name="radix-icons:arrow-up" class="fade-in-100 fade-out-0" size="35" />
      <Icon v-else name="radix-icons:plus-circled" class="fade-in-100 fade-out-0" size="35" />
    </button>
    <form v-if="isOpenForm" @submit="onSubmit" class="form">
      <UiInput placeholder="Name of the deal" v-model="name" v-bind="nameAttrs" type="text" class="input" />
      <UiInput placeholder="Price" v-model="price" v-bind="priceAttrs" type="text" class="input" />
      <UiInput placeholder="Email" v-model="customerEmail" v-bind="customerEmailAttrs" type="text" class="input" />
      <UiInput placeholder="Company" v-model="customerName" v-bind="customerNameAttrs" type="text" class="input" />

      <button class="btn" :disabled="isPending">
        {{ isPending ? 'Loading...' : 'Create' }}
      </button>
    </form>
  </div>
</template>

<style scoped>
.input {
  @apply border-[#161c26] mb-2 placeholder:text-[#748092] focus:border-border transition-colors;
}

.btn {
  @apply text-xs border py-1 px-2 rounded border-[#161c26] hover:border-[#482c65] transition-colors text-[#aebed5] hover:text-white;
}

.form {
  @apply mb-3 block;
  animation: show 0.3s ease-in-out;
}

@keyframes show {
  from {
    @apply border-[#a252c83d];
    transform: translateY(-35px);
    opacity: 0.4;
  }

  90% {
    @apply border-[#a252c83d];
  }

  to {
    @apply border-transparent;
    transform: translateY(0);
    opacity: 1;
  }
}
</style>
