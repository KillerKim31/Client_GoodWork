<script setup>
import { inject } from 'vue'

const props = defineProps({
    title:  String,
    imgUrl: String,
    items:  Array,
    value:  Number
})

const _value = props.value

const { setValue } = inject('SettingPost')
</script>

<template>
    <div class="flex flex-col px-4">
        <div class="flex mb-2 items-center">
            <img :src="imgUrl" class="w-7" />
            <p class="mx-3 font-semibold text-sm">{{ title }}</p>
        </div>
        <select v-model="_value" @change="setValue(title, _value)"
            class="rounded-md mx-2 pl-4 py-1 hover:bg-slate-50 hover:shadow-lg transition" >
            <option v-for="item in items" :value="item.id">{{item.name}}</option>
        </select>
    </div>
</template>