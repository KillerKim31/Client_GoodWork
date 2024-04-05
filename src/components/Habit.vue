<script setup>
import { inject } from 'vue';

defineProps({
    item: Object
})

const { openSettingsTask } = inject('taskActions');
</script>

<template>
    <div class="flex mt-5 rounded-lg transition hover:ring-1 hover:ring-sky-500 hover:shadow-lg hover:-translate-y-0.5 cursor-pointer">
        <!-- Кнопка для полезной привычки -->
        <div v-bind:class="item.is_positive == true ? 'bg-emerald-500' : 'bg-slate-400'"
            class=" flex justify-center rounded-l-lg p-5 px-2 w-auto cursor-grab">
            <div v-bind:class="item.is_positive == true ? 'hover:bg-slate-300 cursor-pointer' : 'bg-slate-50 cursor-grab'"
                class="flex justify-center content-center size-10 bg-slate-50 rounded-full">
                <b v-bind:class="item.is_positive == true ? 'opacity-100' : 'opacity-50'" class="text-3xl py-auto">+</b>
            </div>
        </div>
        <!-- Контент привычки -->
        <div @click="openSettingsTask(item, 'habit')" class="bg-white py-3 px-6 w-full">
            <h1>{{ item.title }}</h1>
            <p class="text-sm">{{ item.descript }}</p>
        </div>
        <!-- Кнопка для вредной привычки -->
        <div v-bind:class="item.is_positive == false ? 'bg-rose-500' : 'bg-slate-400'"
            class="flex justify-center rounded-r-lg p-5 px-2 w-auto">
            <div v-bind:class="item.is_positive == false ? 'hover:bg-slate-300 cursor-pointer' : 'bg-slate-50 cursor-grab'"
                class="flex justify-center content-center size-10 bg-slate-50 rounded-full">
                <b v-bind:class="item.is_positive == false ? 'opacity-100' : 'opacity-50'" class="text-3xl py-auto">–</b>
            </div>
        </div>
    </div>
</template>