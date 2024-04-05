<script setup>
// Подключение системных библиотек
import { onMounted, provide, ref } from 'vue';
import axios from 'axios';

// Подключение компонентов
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
import ItemList from './components/ItemList.vue'
import Selector from './components/Selector.vue'
import SettingTask from './components/SettingTask.vue'

// URL адрес на API
const URL_API = 'http://localhost:3000/api'

const targets = ref([])     // список целей
const tasks = ref([])       // список задач
const habits = ref([])      // список привычек

const filters = ref({
    classes: [],
    level: '',
    done: true
})

// Событие при загрузке страницы
onMounted(async () => {
    getTaskList()
    getTargetList()
    getHabitList()
})

const getTaskList = async () => {
    try {
        const { data } = await axios.get(URL_API + '/task')
        console.log(data);
        tasks.value = data
    } catch (err) {
        console.log(err)
    }
}

const getTargetList = async () => {
    try {
        const { data } = await axios.get(URL_API + '/target')
        console.log(data);
        targets.value = data
    } catch (err) {
        console.log(err)
    }
}

const getHabitList = async () => {
    try {
        const { data } = await axios.get(URL_API + '/habit')
        console.log(data);
        habits.value = data
    } catch (err) {
        console.log(err)
    }
}

const refreshList = async (label) => {
    try {
        switch (label) {
            case 'task':
                getTaskList()
                break
            case 'target':
                getTargetList()
                break
            case 'habit':
                getHabitList()
                break
        }
    } catch (err) {
        console.log(err)
    }
}

// Удаление записи из таблицы в базе данных
const deleteItem = async (label, id) => {
    try {
        const query = URL_API + `/${label}/` + id
        await axios.delete(query)
        refreshList(label)
    } catch (err) {
        console.log(err)
    }
}

const updateItem = async (label, id, post) => {
    try {
        console.log(post)
        const query = URL_API + `/${label}/` + id
        await axios.put(query, post)
        settingOpen.value = false
        refreshList(label)
    } catch (err) {
        console.log(err)
    }
}

const createItem = async (label, post) => {
    try {
        await axios.post(URL_API + `/${label}/`, post)
    } catch (err) {
        console.log(err)
    }
}

const fetchItems = async (tabel) => {
    try {
        const params = {
            classes: filters.value.classes,
            level:   filters.value.level,
            done:    filters.value.done
        }
        
        switch (tabel) {
            case 'task':
                var { data } = await axios.get(URL_API + '/task/done/' + params.done)
                tasks.value = data
                refreshList('task')
                console.log(data);
                console.log(URL_API + '/task/done/' + params.done);
                break
            case 'target':
                var { data } = await axios.get(URL_API + '/target/done/' + params.done)
                targets.value = data
                refreshList('target')
                break
            case 'habit':
                var { data } = await axios.get(URL_API + '/habit/done/' + params.done)
                habits.value = data
                refreshList('habit')
                break
        }
    } catch (err) {
        console.log(err)
    }
}

const settingOpen = ref(false)
const activeItem = ref([])
const activeTabel = ref()

const openSettingsTask = (item, tabel) => {
    settingOpen.value = true
    activeItem.value = item
    activeTabel.value = tabel
    console.log(`settingOpen = ${settingOpen.value}`)
}

const closeSettingsTask = () => {
    settingOpen.value = false;
    console.log(`settingOpen = ${settingOpen.value}`)
}

provide('taskActions', {
    openSettingsTask,
    closeSettingsTask
})

provide('ApiTools', {
    URL_API,
    fetchItems,
    refreshList,
    deleteItem,
    createItem,
    updateItem,
    createItem
})

</script>

<template>
    <div>
        <SettingTask v-if="settingOpen" :item="activeItem" :tabel="activeTabel" />
        <Header />
        <div class="bg-blue-600 flex items-center justify-between h-44 pl-10">
            <h1 class="text-2xl font-light text-white italic">“Неважно, как медленно вы идете, главное — не останавливаться”
            </h1>
            <div class="flex items-center">
                <div class="flex flex-col items-end">
                    <h2 class="uppercase">Руслан</h2>
                    <p>Уровень 3</p>
                    <div class="flex items-center mt-5">
                        <p>20 / 50</p>
                        <div class="square mx-3"></div>
                        <img src="./assets/icons/XP.png" class="w-8" />
                    </div>
                </div>
                <div class="size-40 bg-slate-300 border-8 border-gray-700 ml-10 mr-5">
                    <img src="./assets/icons/avatar.png" />
                </div>
            </div>
        </div>
        <div class="flex justify-end pt-5 pr-10">
            <Selector name="Выбор цели" imgUrl="src/assets/icons/target.png" />
            <Selector name="Класс задач" imgUrl="src/assets/icons/class.png" />
        </div>
        <div class="flex justify-center gap-10 bg-bg-slate-50 mx-20 my-5 taskListBox">
            <ItemList type="Цели"       :isTask="true"  :items="targets" />
            <ItemList type="Задачи"     :isTask="true"  :items="tasks" />
            <ItemList type="Привычки"   :isTask="false" :items="habits" />
        </div>
        <Footer />
    </div>
</template>

<style>
.square {
    width: 300px;
    height: 20px;
    background: #203154;
}

.taskListBox {
    height: 580px;
}
</style>
