<script setup lang="ts">
import { ref, reactive, toRaw } from 'vue';
import SquareCell from './components/SquareCell.vue';
import { puzzles, solutions } from './squares';
import PaletteCircle from './components/PaletteCircle.vue';
import HelpButton from './components/HelpButton.vue';
import ResetButton from './components/ResetButton.vue';

const GRID_SIZE = 7;
const CELL_SIZE = 10;

const birthday = new Date('2024-03-14T00:00:00');
const daysSinceBirth = Math.floor((Date.now() - birthday.getTime()) / (1000 * 60 * 60 * 24));
// console.log(birthday, daysSinceBirth);
const puzzleIndex = daysSinceBirth;

const original = puzzles[puzzleIndex % puzzles.length];
// Creating deep copy
var puzzle = reactive([...original]);
const solution = solutions[puzzleIndex];

var selectedColour = ref(0);

const updateSelectedColour = (newSelectedColour: number) => {
  selectedColour.value = newSelectedColour;
};

const updateCell = (cellIndex: number, newValue: number) => {
  // Don't update cells which were in the original puzzle
  if (original[cellIndex] != -1) {
    return;
  }
  var cellValue = puzzle[cellIndex];

  if (cellValue != -1) {
    puzzle[cellIndex] = -1;
  } else {
    puzzle[cellIndex] = newValue;
  }

  checkForWin();
};

const checkForWin = () => {
  // Direct comparison does not work for some reason
  for (let i = 0; i < toRaw(puzzle).length; i++) {
    if (toRaw(puzzle)[i] != solution[i]) {
      return;
    }
  }
  // Disable pointer interaction
  const grid = document.getElementById('grid')!;
  const resetButton = document.getElementById('resetButton')!;

  grid.style.pointerEvents = 'none';
  resetButton.style.pointerEvents = 'none';

  winGame();
};

const winGame = () => {
  const elements = document.getElementsByClassName('cell') as HTMLCollection;

  for (let i = 0; i < elements.length; i++) {
    const element = elements[i];
    const style = getComputedStyle(document.documentElement);

    element.animate(
      [
        {
          offset: 1,
          transform: 'rotate(180deg)'
        }
      ],
      {
        duration: 500,
        delay: (i / elements.length) * 500,
        easing: 'ease-out',
        fill: 'forwards'
      }
    );
    element.animate(
      [
        {
          backgroundColor: style.getPropertyValue('--theme-colour-' + (puzzle[i] % GRID_SIZE))
        },
        {
          backgroundColor: style.getPropertyValue('--theme-colour-' + ((puzzle[i] + 1) % GRID_SIZE))
        },
        {
          backgroundColor: style.getPropertyValue('--theme-colour-' + ((puzzle[i] + 2) % GRID_SIZE))
        },
        {
          backgroundColor: style.getPropertyValue('--theme-colour-' + ((puzzle[i] + 3) % GRID_SIZE))
        },
        {
          backgroundColor: style.getPropertyValue('--theme-colour-' + ((puzzle[i] + 4) % GRID_SIZE))
        },
        {
          backgroundColor: style.getPropertyValue('--theme-colour-' + ((puzzle[i] + 5) % GRID_SIZE))
        },
        {
          backgroundColor: style.getPropertyValue('--theme-colour-' + ((puzzle[i] + 6) % GRID_SIZE))
        },
        {
          backgroundColor: style.getPropertyValue('--theme-colour-' + ((puzzle[i] + 7) % GRID_SIZE))
        }
      ],
      { iterations: 2, duration: 2000 }
    );
  }

  showWin();
};

const resetGrid = () => {
  for (let i = 0; i < puzzle.length; i++) {
    puzzle[i] = original[i];
  }
};

const getWinDialog = () => document.getElementById('winDialog') as HTMLDialogElement;

const showWin = () => {
  getWinDialog().showModal();
};

const closeWin = (event: MouseEvent) => {
  const winDialog = getWinDialog();
  if (event.target === winDialog) {
    winDialog.close();
  }
};
const cellCallback = (index: number) => {
  updateCell(index, selectedColour.value);
};

const paletteCallback = (index: number) => {
  updateSelectedColour(index);
};
</script>

<template>
  <header>
    <div id="title">Quadradle</div>
    <div
      style="
        margin: auto;
        max-width: 500px;
        display: flex;
        align-items: center;
        justify-content: space-between;
      "
    >
      <ResetButton @click="resetGrid" />
      <div width="200px" style="font-size: 24px; text-align: center">#{{ puzzleIndex + 1 }}</div>
      <HelpButton />
    </div>
  </header>
  <main>
    <br />
    <dialog id="winDialog" @click="closeWin">
      <div style="display: flex; flex-direction: column">
        <div style="display: flex">
          <b style="font-size: 30px; text-align: end">Quadradle complete!</b>
          <a
            class="material-symbols-outlined"
            @click="
              () => {
                getWinDialog().close();
              }
            "
            style="cursor: pointer; margin-left: auto"
            >close</a
          >
        </div>
      </div>
    </dialog>
    <div id="grid">
      <div v-for="i in puzzle.length" :key="i">
        <SquareCell
          @clicked-cell="cellCallback"
          :size="CELL_SIZE"
          :index="i - 1"
          :value="puzzle[i - 1]"
        />
      </div>
    </div>
    <br />
    <div id="palette">
      <div v-for="i in GRID_SIZE" :key="i">
        <PaletteCircle
          @clicked-palette="paletteCallback"
          :size="CELL_SIZE"
          :index="i - 1"
          :style="
            i - 1 == selectedColour
              ? {
                  outlineStyle: 'solid',
                  outlineColor: '#f5e0dc',
                  outlineWidth: '5px'
                }
              : {}
          "
        />
      </div>
    </div>
  </main>
</template>
