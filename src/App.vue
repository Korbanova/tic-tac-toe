<script setup>
import Board from "./components/Board.vue";
import Score from "./components/Score.vue";
import {ref} from 'vue';
import {Modal} from 'usemodal-vue3';

const isPopupVisible = ref(false);

const score = ref({
  user: 0,
  bot: 0,
  draw: 0
});

const childBoard = ref(null);
const gameResult = ref('');

function endHandler(winner) {
  switch (winner) {
    case 'user':
      score.value.user++;
      gameResult.value = "Вы победили!";
      break;
    case 'bot':
      score.value.bot++;
      gameResult.value = "Вы проиграли!";
      break;
    case 'draw':
      score.value.draw++;
      gameResult.value = "Ничья!";
      break;
  }

  openPopup();
}

function closePopup() {
  isPopupVisible.value = false;
}

function openPopup() {
  isPopupVisible.value = true;
}

function onClosePopup() {
  if (childBoard.value) {
    childBoard.value.endGame();
  }
}
</script>

<template>
  <div class="title">
    Крестики-нолики
  </div>
  <div class="game">
    <Board @end="endHandler" ref="childBoard"/>
    <Score :score="score"/>
  </div>

  <Modal v-model:visible="isPopupVisible"
         :type="'clean'"
         modalClass="'popup'"
         @onUnVisible="onClosePopup">
    <span class="close" @click="closePopup">&times;</span>
    <div class="res">{{ gameResult }}</div>
    <button class="button" @click="closePopup">Новая игра</button>
  </Modal>
</template>

<style>
.title {
  margin-top: 50px;
  font-size: 40px;
  color: white;
  text-align: center;
}

.game {
  margin: 50px auto;
  max-width: 324px;
}

.modal-vue3-content {
  padding: 20px;
  max-width: 250px;
  border-radius: 5px;
  border: 0;
  background-color: #2b9595;
  text-align: center;
  color: white;
}

.modal-vue3-content .close {
  position: absolute;
  display: block;
  right: 5px;
  top: -5px;
  font-size: 24px;
  cursor: pointer;
}

.modal-vue3-content .button {
  padding: 10px 20px;
  margin-top: 10px;
  border-radius: 5px;
  border: 0;
  background-color: black;
  color: white;
  cursor: pointer;
}
</style>
