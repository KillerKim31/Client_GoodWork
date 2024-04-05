<script>
export default {
  data() {
    return {
      isListOpen: false
    };
  },
  methods: {
    showList() {
      this.isListOpen = true;
    },
    hideList() {
      this.isListOpen = false;
    }
  }
};
</script>

<template>
  <header class="bg-gradient-to-r from-sky-500 to-indigo-500 flex justify-between px-8">
    <div class="flex gap-20">
      <div class="flex items-center gap-3">
        <img src="../assets/icons/logo.png" alt="logo" class="w-20" />
        <h1 class="text-3xl font-bold text-white">GoodWork</h1>
      </div>
      <ul class="flex items-center gap-20 text-xl text-white">
        <li>
          <a href="#" class="pb-2 hover:border-b-4 rounded-lg">Задачи</a>
        </li>
        <li>
          <a href="#" class="pb-2 hover:border-b-4 rounded-lg">Инвентарь</a>
        </li>
        <li>
          <a href="#" class="pb-2 hover:border-b-4 rounded-lg">Магазин</a>
        </li>
        <div class="flex flex-col">
          <div @mouseover="showList()" @mouseleave="hideList()" class="cursor-pointer"></div>
          <a href="#" class="pb-2 hover:border-b-4 rounded-lg">Помощь</a>
          <ul v-if="isListOpen">
            <li>Элемент списка 1</li>
            <li>Элемент списка 2</li>
            <li>Элемент списка 3</li>
          </ul>
        </div>
      </ul>
    </div>
    <div class="flex items-center gap-40">
      <div class="flex items-center gap-3">
        <img src="../assets/icons/coin.png" alt="coin icon" class="w-8" />
        <h1 class="text-lg text-white">125.5</h1>
      </div>
      <img src="../assets/icons/person.png" alt="person icon" class="cursor-pointer w-8 hover:brightness-110" />
    </div>
  </header>
</template>

<style scoped></style>
