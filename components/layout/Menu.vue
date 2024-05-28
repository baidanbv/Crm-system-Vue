<script setup lang="ts">
import { MENU_DATA } from './menu.data';
</script>

<template>
  <div>
    <NuxtLink 
    class="flex items-center gap-2 py-1 px-3 rounded-lg w-full text-secondary-foreground hover:bg-gray-700 hover:shadow transition-all mb-2" 
    v-for="item in MENU_DATA" 
    :key="item.name" 
    :to="item.url">
    <Icon :name="item.icon" size="24" /> 
    {{ item.name }}
     </NuxtLink>
  </div>
</template>
