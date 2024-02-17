<script setup lang="ts">
import { onMounted, ref, watch } from "vue";
import axios from "axios";
import Uznai from "./components/uznai.vue";
import Ru from "./components/ru.vue";
import En from "./components/en.vue";
import Bg from "./components/bg.vue";
import ElectraRu from "./assets/Electra - TECHNIC[RU].wav";
import ElectraEn from "./assets/Electra - TECHNIC [ENG].wav";

const selected = ref(0);
const audio = ref([new Audio(ElectraRu), new Audio(ElectraEn)]);

onMounted(() => {
  audio.value.forEach(a => {
    a.onended = () => onClickBlock(0);
  });
});

const sendCommandToVMix = (functionName, inputNumber) => {
  // Замените адрес на актуальный адрес вашего vMix HTTP API
  const vmixApiUrl = `http://10.30.40.65:8088/api`;

  axios.get(`${vmixApiUrl}?Function=${functionName}&Input=${inputNumber}`)
      .then(response => {
        console.log('vMix command response:', response.data);
      })
      .catch(error => {
        console.error('Error sending command to vMix:', error);
      });
};

watch(() => selected.value, (id) => {
  audio.value.forEach(a => {
    a.pause();
    a.currentTime = 0;
  });

  switch (id) {
    case 1:
      console.log("Нажата кнопка 1");
      // Отправка HTTP запроса в vMix
      sendCommandToVMix('Cut', '3');
      break;
    case 2:
      console.log("Нажата кнопка 2");
      // Отправка HTTP запроса в vMix
      sendCommandToVMix('Cut', '2');
      break;
    case 3:
      console.log("Нажата кнопка 3");
      // audio.value[1].play();
      // Отправка HTTP запроса в vMix
      sendCommandToVMix('Cut', '1');
      break;
  }
});

const onClickContainer = () => {
  selected.value = 0;
  sendCommandToVMix('Cut', '1');
};

const onClickBlock = (id: number) => {
  if (selected.value === id) {
    selected.value = 0;
  } else {
    selected.value = id;
  }
};
document.addEventListener('contextmenu', event => event.preventDefault());
</script>

<template>
  <div class="container" @click="onClickContainer">
    <div class="blocks">
      <div class="block" :class="selected == 1 ? 'selected' : ''" @click.stop="onClickBlock(1)">
        <Bg class="bg"/>
        <Uznai class="text"/>
      </div>
      <div class="block" :class="selected == 2 ? 'selected' : ''" @click.stop="onClickBlock(2)">
        <Bg class="bg"/>
        <Ru class="text"/>
      </div>
      <div class="block" :class="selected == 3 ? 'selected' : ''" @click.stop="onClickBlock(3)">
        <Bg class="bg"/>
        <En class="text"/>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
