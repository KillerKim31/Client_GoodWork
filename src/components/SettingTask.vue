<script setup>
import { inject, onMounted, provide, ref } from 'vue';
import axios from 'axios';

import ElemSetting from './ElemSetting.vue'

const props = defineProps({
    item:   Object,
    tabel:  String
})

const classes = ref([])
const levels = ref([])

// Событие при запуске окна настройки
onMounted(async () => {
    getclassList()
    getlevelsList()
})

const choiceWindow = () => {
    if (confirm("Вы точно хотите удалить задачу?")) {
        deleteItem(props.tabel, props.item.id)
        closeSettingsTask()
    }
}

const buildPost = () => {
    const post = props.item
    return post
}

const getclassList = async () => {
    try {
        const { data } = await axios.get(URL_API + '/classTask')
        classes.value = data
    } catch (err) {
        console.log(err)
    }
}

const getlevelsList = async () => {
    try {
        const { data } = await axios.get(URL_API + '/levelTask')
        levels.value = data
    } catch (err) {
        console.log(err)
    }
}

const getDate = (_date) => {
    const dt = new Date(_date)

    var day = dt.getDate();
    var month = dt.getMonth() + 1;
    var year = dt.getFullYear();

    const formattedDate = `${year}-${(month + 1).toString().padStart(2, '0')}-${day.toString().padStart(2, '0')}`
    return formattedDate
}

const setValue = (title, value) => {
    switch (title) {
        case "Класс":
            props.item.ClassTaskId = value
            break
        case "Сложность":
            props.item.LevelTaskId = value
            break
        case "Срок выполнения":
            console.log(value);
            props.item.date_completed = value
            break
    }
}

const _date = getDate(props.item.date_completed)

const { closeSettingsTask } = inject("taskActions")
const { URL_API, deleteItem, updateItem } = inject('ApiTools')

provide('SettingPost', {
    setValue
})
</script>

<template>
    <div class="modal">
        <div class="flex h-full place-items-center justify-center">
            <div v-if="tabel == 'task' || tabel == 'target'" class="flex flex-col window">
                <div class="flex flex-col bg-amber-400 h-1/4 rounded-t-2xl p-5">
                    <div class="flex justify-between">
                        <h1 v-if="tabel == 'task'" class="text-3xl font-bold">Задача</h1>
                        <h1 v-if="tabel == 'target'" class="text-3xl font-bold">Цель</h1>
                        <div class="flex gap-10 justify-between">
                            <button @click="closeSettingsTask" class="hover:underline">Отмена</button>
                            <button @click="updateItem('task', item.id, buildPost())"
                                class="font-medium bg-slate-50 shadow-md rounded-md transition hover:shadow-xl hover:bg-white px-3 py-1">Сохранить</button>
                        </div>
                    </div>
                    <h2 class="my-2 text-sm font-semibold">Заголовок *</h2>
                    <input v-model="props.item.title"
                        class="rounded-md px-3 py-1 border outline-none hover:border-gray-400 hover:bg-slate-50">
                </div>
                <div class="flex flex-col bg-slate-100 h-max rounded-b-2xl p-5">
                    <h2 class="font-semibold mb-2">Описание</h2>
                    <textarea v-model="props.item.descript"
                        class="text-sm text-gray-600 align-left h-18 rounded-md px-3 py-1 border outline-none hover:border-gray-400 hover:bg-slate-50"></textarea>
                    <div class="flex flex-col gap-5 mt-7 mb-10">

                        <ElemSetting 
                            title="Сложность"
                            imgUrl="src/assets/icons/level.png"
                            :items="levels"
                            :value="props.item.LevelTaskId"/>

                        <ElemSetting 
                            title="Класс"
                            imgUrl="src/assets/icons/class.png"
                            :items="classes"
                            :value="props.item.ClassTaskId" />

                        <div class="flex flex-col px-4">
                            <div class="flex mb-2 items-center">
                                <img src="../assets/icons/calendar.png" class="w-7" />
                                <p class="mx-3 font-semibold text-sm">Срок выполнения</p>
                            </div>
                            <input @change="setValue('Срок выполнения', _date)" 
                                v-model="_date"
                                type="date"
                                class="rounded-md mx-2 pl-4 py-1 hover:bg-slate-50 hover:shadow-lg transition" />
                        </div>
                    </div>
                    <div @click="choiceWindow"
                        class="flex items-center justify-center gap-3 mb-5 text-lg text-rose-600 cursor-pointer hover:underline">
                        <img src="../assets/icons/trash.png" class="w-7" />
                        <p v-if="tabel == 'task'">Удалить задачу</p>
                        <p v-else>Удалить цель</p>
                    </div>
                </div>
            </div>
            
            <div v-if="tabel == 'habit'" class="flex flex-col window">
                <div class="flex flex-col bg-amber-400 h-1/4 rounded-t-2xl p-5">
                    <div class="flex justify-between">
                        <h1 class="text-3xl font-bold">Привычка</h1>
                        <div class="flex gap-10 justify-between">
                            <button @click="closeSettingsTask" class="hover:underline">Отмена</button>
                            <button @click="updateItem('habit', item.id, buildPost())"
                                class="font-medium bg-slate-50 shadow-md rounded-md transition hover:shadow-xl hover:bg-white px-3 py-1">Сохранить</button>
                        </div>
                    </div>
                    <h2 class="my-2 text-sm font-semibold">Заголовок *</h2>
                    <input v-model="props.item.title"
                        class="rounded-md px-3 py-1 border outline-none hover:border-gray-400 hover:bg-slate-50">
                </div>
                <div class="flex flex-col bg-slate-100 h-max rounded-b-2xl p-5">
                    <h2 class="font-semibold mb-2">Описание</h2>
                    <textarea v-model="props.item.descript"
                        class="text-sm text-gray-600 align-left h-18 rounded-md px-3 py-1 border outline-none hover:border-gray-400 hover:bg-slate-50"></textarea>
                    <div class="flex flex-col gap-5 mt-7 mb-10">

                        <ElemSetting 
                            title="Сложность"
                            imgUrl="src/assets/icons/level.png"
                            :items="levels"
                            :value="props.item.LevelTaskId"/>

                        <ElemSetting 
                            title="Класс"
                            imgUrl="src/assets/icons/class.png"
                            :items="classes"
                            :value="props.item.ClassTaskId" />
                    </div>
                    <div @click="choiceWindow"
                        class="flex items-center justify-center gap-3 mb-5 text-lg text-rose-600 cursor-pointer hover:underline">
                        <img src="../assets/icons/trash.png" class="w-7" />
                        <p>Удалить привычку</p>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(30, 58, 138, 0.9);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 10;
}

.window {
    height: 65%;
    width: 450px;
}
</style>