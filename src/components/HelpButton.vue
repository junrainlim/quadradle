<script setup lang="ts">
const getHelpDialog = () => document.getElementById('helpDialog') as HTMLDialogElement;

const showHelp = () => {
  getHelpDialog().showModal();
};

const closeHelp = (event: MouseEvent) => {
  const helpDialog = getHelpDialog();
  if (event.target === helpDialog) {
    helpDialog.close();
  }
};

const helpText =
  'Use the palette below the grid to select a colour, then click on a cell to fill it. The goal of the game is to fill every empty cell with one of the seven colours. Each row and column can only contain one of each colour (think Sudoku). Every puzzle has a unique solution, and a new puzzle is released every day at 12:00 AM (local time).';
</script>
<template>
  <dialog id="helpDialog" @click="closeHelp">
    <div style="display: flex; flex-direction: column">
      <div style="display: flex">
        <b style="font-size: 30px; text-align: end">How to play:</b>
        <a
          class="material-symbols-outlined"
          @click="
            () => {
              getHelpDialog().close();
            }
          "
          style="cursor: pointer; margin-left: auto"
          >close</a
        >
      </div>
      {{ helpText }}
    </div>
  </dialog>
  <a id="helpButton" class="material-symbols-outlined" @click="showHelp">help</a>
</template>
