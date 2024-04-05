<script setup>
defineProps({
  name:   String,
  imgUrl: String,
  items:  Array
})
</script>

<template>
  <div class="relative">
    <img :src="imgUrl" class="absolute w-5 left-4 top-2" />
    <select class="border rounded-md mx-2 pl-8 py-1 hover:bg-slate-50 hover:shadow-lg transition" name="target"
      id="target-select">
      <option value="">{{ name }}</option>
      <option v-for="item in items" value="item.">Dog</option>
      <option value="cat">Cat</option>
      <option value="hamster">Hamster</option>
    </select>
  </div>
</template>