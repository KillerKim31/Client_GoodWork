<script setup>
import { inject } from 'vue'

import Task from './Task.vue'
import Habit from './Habit.vue'

defineProps({
  type:   String,
  hint:   String,
  items:  Object,
  tabs:   Array,
})

const { fetchItems, refreshList, createItem } = inject('ApiTools')
</script>

<template>
  <div class="bg-slate-300 w-2/4 h-full rounded-b-2xl">
    <div class="flex justify-between items-center px-5 bg-blue-900">
      <h1 class="text-xl font-medium text-white">{{ type }}</h1>

      <div v-if="type == 'Цели' || type == 'Задачи'" class="flex justify-center gap-5">
        <h2 @click="refreshList('task')" class="cursor-pointer text-white hover:text-amber-300">Все</h2>
        <h2 @click="fetchItems('task')" class="cursor-pointer text-white hover:text-amber-300">Достигнутые</h2>
      </div>

      <!-- <div v-else class="flex justify-center gap-5">
        <h2 class="cursor-pointer text-white hover:text-amber-300">Все</h2>
        <h2 class="cursor-pointer text-white hover:text-amber-300">Слабые</h2>
        <h2 class="cursor-pointer text-white hover:text-amber-300">Сильные</h2>
      </div> -->

    </div>
    <div class="flex flex-col p-3">
      <input v-on:keyup.enter="createItem(type == 'Цели' ? 'target' : 'task', )"
        class="w-full border border-gray-300 rounded-md p-2 outline-none hover:border-gray-400 hover:bg-slate-50"
        placeholder="Добавить привычку">

      <Task   v-if="type == 'Цели'"     v-for="item in items" :item="item" type="Цели"/>
      <Task   v-if="type == 'Задачи'"   v-for="item in items" :item="item" type="Задачи"/>
      <Habit  v-if="type == 'Привычки'" v-for="item in items" :item="item" />
    </div>
  </div>
</template>