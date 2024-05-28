<script lang="ts" setup>
import { useAuthStore, useIsLoadingStore } from '~/store/auth.store';

const store = useAuthStore();
const isLoadingStore = useIsLoadingStore();
const router = useRouter();

const logout = async () => {
  isLoadingStore.set(true);
  await account.deleteSession('current');
  store.clear();
  await router.push('/login');
  isLoadingStore.set(false);
};
</script>

<template>
  <aside class="px-5 py-8 bg-sidebar h-full relative w-full max-w-[350px] ">
    <NuxtLink to="/" class="mb-10 flex justify-center">
      <NuxtImg src="/logo.svg" alt="srm-system" width="200px" />
    </NuxtLink>
    <button @click="logout" class="absolute top-1 right-2 transition-colors text-primary-foreground hover:text-primary">
      <Icon name="line-md:logout" size="24" />
    </button>
    <LayoutMenu />
  </aside>
</template>
