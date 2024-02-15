<script setup lang="ts">
import {onMounted, ref, watch} from "vue";
import type {Ref} from "vue";
import Ru from "./components/ru.vue";
import En from "./components/en.vue";
import Bg from "./components/bg.vue";
import ElectraRu from "./assets/Electra - TECHNIC[RU].wav";
import ElectraEn from "./assets/Electra - TECHNIC [ENG].wav";

const selected = ref(0);
const audio:Ref<HTMLAudioElement[]> = ref([new Audio(ElectraRu), new Audio(ElectraEn)]);

onMounted(()=>{
  audio.value.forEach(a=>{
    a.onended = ()=>onClickBlock(0)
  })
})

watch(() => selected.value, (id: typeof selected.value) => {
  audio.value.forEach(a=>{
    a.pause()
    a.currentTime = 0
  })

  switch (id) {
    case 1: {
      console.log("Нажата кнопка 1")
      audio.value[0].play()
      break;
    }
    case 2: {
      console.log("Нажата кнопка 2")
      audio.value[1].play()
      break;
    }
  }
})

const onClickContainer = () => {
  selected.value = 0
}

const onClickBlock = (id: number) => {
  if (selected.value === id) {
    selected.value = 0
  } else {
    selected.value = id
  }
}
</script>

<template>
  <div class="container" @click="onClickContainer">
    <div class="blocks">
      <div class="block" :class="selected == 1 ? 'selected' : ''" @click.stop="onClickBlock(1)">
        <Bg class="bg"/>
        <Ru class="text"/>
      </div>
      <div class="block" :class="selected == 2 ? 'selected' : ''" @click.stop="onClickBlock(2)">
        <Bg class="bg"/>
        <En class="text"/>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
