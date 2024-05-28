<script setup lang="ts">
import { useAuthStore, useIsLoadingStore } from '~/store/auth.store';
import { v4 as uuid } from 'uuid';
useHead({
  title: 'Login | CRM System'
});

const emailRef = ref('');
const passwordRef = ref('');
const nameRef = ref('');

const isLoadingStore = useIsLoadingStore();
const authStore = useAuthStore();
const router = useRouter();

const login = async () => {
  isLoadingStore.set(true);
  await account.createEmailPasswordSession(emailRef.value, passwordRef.value);
  const response = await account.get();
  if (response) {
    authStore.set({
      email: response.email,
      name: response.name,
      status: response.status
    });
  }

  emailRef.value = '';
  passwordRef.value = '';
  nameRef.value = '';

  await router.push('/');
  isLoadingStore.set(false);
};

const register = async () => {
  await account.create(uuid(), emailRef.value, passwordRef.value, nameRef.value);
  await login();
};
</script>

<template>
  <div class="flex items-center justify-center min-h-screen w-full">
    <div class="rounded bg-sidebar w-1/3 p-5">
      <h1 class="font-bold text-2xl mb-10 text-center text-secondary-foreground">Login</h1>

      <form>
        <UiInput v-model="emailRef" label="Email" placeholder="Enter your email" type="email" class="mb-3" />
        <UiInput v-model="passwordRef" label="Password" placeholder="Enter your password" type="password" class="mb-3" />
        <UiInput v-model="nameRef" label="name" placeholder="Enter your name" type="text" class="mb-3" />
        <div>
          <UiButton type="button" class="w-full mt-5 cursor-pointer" @click="login">Login</UiButton>
          <UiButton type="button" class="w-full mt-5 cursor-pointer" @click="register">Register</UiButton>
        </div>
      </form>
    </div>
  </div>
</template>
