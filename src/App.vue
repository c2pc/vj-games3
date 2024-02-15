<script setup lang="ts">
import {Ref, ref, watch} from "vue";
import Ru from "./components/ru.vue";
import En from "./components/en.vue";
import Bg from "./components/bg.vue";
import ElectraRu from "./assets/Electra - TECHNIC[RU].wav";
import ElectraEng from "./assets/Electra - TECHNIC [ENG].wav";

const selected = ref(0);
const audio:Ref<HTMLAudioElement | undefined> = ref();

watch(() => selected.value, (id: typeof selected.value) => {
  if (audio.value){
    audio.value?.pause()
    audio.value = undefined
  }

  switch (id) {
    case 1: {
      console.log("Нажата кнопка 1")
      audio.value = new Audio(ElectraRu)
      break;
    }
    case 2: {
      console.log("Нажата кнопка 2")
      audio.value = new Audio(ElectraEng)
      break;
    }
  }

  if (audio.value){
    audio.value?.play()
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
