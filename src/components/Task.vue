<script setup>
import { inject } from 'vue';

defineProps({
    item: Object,
    type: String
})

const { openSettingsTask } = inject('taskActions')
const color = "#FF8C00"
</script>

<template>
    <div @click="openSettingsTask(item, type == 'Цели' ? 'target' : 'task')"
        class="flex mt-5 min-h-16 rounded-lg transition hover:ring-1 hover:ring-sky-500 hover:shadow-lg hover:-translate-y-0.5 cursor-pointer">
        <div class="flex justify-center rounded-l-lg pt-4 px-2 w-auto" style="background-color: #FF8C00">
            <div class="relative size-8 bg-slate-100 rounded hover:bg-white cursor-pointer">
                <img v-if="item.isDone" src="../assets/icons/checker.png" class="absolute left-1 top-1 w-6 opacity-80" />
            </div>
        </div>
        <div class="bg-white rounded-r-lg w-full py-3 px-6 ">
            <h1>{{ item.title }}</h1>
            <p class="text-sm">{{ item.descript }}</p>
        </div>
    </div>
</template>

<style></style>