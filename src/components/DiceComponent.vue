<template>
  <div class="dice-roller">
    <div class="dice" @click="rollDice" :class="diceClasses">
      <template v-if="diceName === 'd100'">
        <div
          v-for="(result, index) in ['tens', 'units']"
          :key="index"
          class="d100-die"
          :class="{ rollAnimation: isRolling }"
        >
          <img class="dice-icon" src="/d10.svg" :alt="`${result} die`" />
          <span class="roll-result" :class="result">{{
            d100Results[result]
          }}</span>
        </div>
      </template>
      <template v-else>
        <img class="dice-icon" :src="`/${diceName}.svg`" alt="Dice icon" />
        <span class="roll-result" :class="{ lowerPosition: isWrongSizing }">{{
          rollResult
        }}</span>
      </template>
    </div>
  </div>
  <div class="dice-selection">
    <button
      class="select-button"
      id="left"
      @click="changeDice(-1)"
      :disabled="currentDiceIndex === 0"
    >
      <Icon class="arrow-left" icon="ep:arrow-up" rotate="270deg" />
    </button>
    <span v-if="diceName !== 'd'" class="dice-name">{{ diceName }}</span>
    <div v-else class="custom-dice-input">
      <span class="d-prefix">d</span>
      <input
        v-model.number="customSides"
        type="number"
        class="custom-sides-input"
        @input="handleCustomSidesInput"
        ref="customInput"
      />
    </div>
    <button
      class="select-button"
      id="right"
      @click="changeDice(1)"
      :disabled="currentDiceIndex === diceNames.length - 1"
    >
      <Icon class="arrow-right" icon="ep:arrow-up" rotate="90deg" />
    </button>
  </div>
  <button class="roll-button" @click="rollDice">Roll</button>
</template>

<script setup>
import { ref, computed, onMounted, watch } from "vue";

const diceNames = ["d2", "d4", "d6", "d8", "d10", "d12", "d20", "d100", "d"];
const rollResult = ref(1);
const isChangingDice = ref(false);
const isRolling = ref(false);
const currentDiceIndex = ref(0);
const customSides = ref(20);
const customInput = ref(null);
const d100Results = ref({ tens: "00", units: "00" });

const diceName = computed(() => diceNames[currentDiceIndex.value]);
const isWrongSizing = computed(() => ["d12", "d4"].includes(diceName.value));

const diceClasses = computed(() => ({
  changeDiceAnimation: isChangingDice.value,
  rollAnimation:
    isRolling.value && diceName.value !== "d2" && diceName.value !== "d100",
  flipAnimation: isRolling.value && diceName.value === "d2",
}));

onMounted(() => {
  diceNames.forEach((dice) => {
    new Image().src = `/${dice}.svg`;
  });
  resizeInput();
});

watch(customSides, resizeInput);

function resizeInput() {
  if (customInput.value) {
    const minWidth = 40;
    const inputWidth = Math.max(
      minWidth,
      customSides.value.toString().length * 15
    );
    customInput.value.style.width = `${inputWidth}px`;
  }
}

function toggleAnimation(type, duration) {
  const animationState = type === "change" ? isChangingDice : isRolling;
  animationState.value = true;
  setTimeout(() => (animationState.value = false), duration);
}

function changeDice(direction) {
  if (isChangingDice.value || isRolling.value) return;
  toggleAnimation("change", 165);
  currentDiceIndex.value += direction;
  rollResult.value = 1;
}

function rollDice() {
  if (isChangingDice.value || isRolling.value) return;
  const isD2 = diceName.value === "d2";
  toggleAnimation(isD2 ? "flip" : "roll", isD2 ? 1000 : 500);

  setTimeout(
    () => {
      if (diceName.value === "d") {
        rollResult.value = Math.floor(Math.random() * customSides.value + 1);
      } else if (diceName.value === "d100") {
        d100Results.value = {
          tens: Math.floor(Math.random() * 10)
            .toString()
            .padStart(2, "0"),
          units: Math.floor(Math.random() * 10)
            .toString()
            .padStart(2, "0"),
        };
        rollResult.value = parseInt(
          d100Results.value.tens + d100Results.value.units
        );
      } else {
        rollResult.value = Math.floor(
          Math.random() * parseInt(diceName.value.slice(1)) + 1
        );
      }
    },
    isD2 ? 500 : 250
  );
}

function handleCustomSidesInput() {
  customSides.value = Math.max(2, Math.min(customSides.value, 1000));
}
</script>

<style scoped lang="scss">
.dice-roller {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row-reverse;
  user-select: none;

  .dice {
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    padding: 10px;
    cursor: pointer;
    -webkit-tap-highlight-color: transparent;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    color: var(--text-color);

    .d100-die {
      display: flex;
      justify-content: center;
      align-items: center;
      position: relative;
      &.rollAnimation {
        animation: roll 0.5s ease-in-out;
      }
    }

    &.rollAnimation {
      animation: roll 0.5s ease-in-out;
    }

    &.flipAnimation {
      animation: flip 1s ease-in-out;
    }

    &.changeDiceAnimation {
      animation: dice-animation 0.2s ease-in-out;
    }

    .dice-icon {
      width: 150px;
      filter: invert(0) contrast(0.6);

      .dark & {
        filter: invert(1); 
      }
    }

    .roll-result {
      position: absolute;
      font-size: 42px;
      font-weight: 800;
      text-align: center;
      width: 100%;
      left: 0;
      right: 0;

      &.lowerPosition {
        transform: translateY(7px);
      }

      &.tens,
      &.units {
        width: auto;
      }
    }
  }
}

@keyframes roll {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotateZ(360deg);
  }
}

@keyframes flip {
  0% {
    transform: rotateY(0);
  }
  100% {
    transform: rotateY(720deg);
  }
}

@keyframes dice-animation {
  0% {
    transform: translateY(-5%);
  }
  50% {
    transform: translateY(5%);
  }
}

.dice-selection {
  display: flex;
  justify-content: center;
  align-items: center;
  color: var(--text-color);

  .select-button {
    height: 30px;
    width: 30px;
    border-radius: 50%;
    margin: 0 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;

    .arrow-left {
      margin-right: 2px;
    }
    .arrow-right {
      margin-left: 2px;
    }

    &.select-button[disabled] {
      opacity: 0.5;
      cursor: default;
      &:hover {
        background: none;
      }
    }
  }

  .dice-name {
    font-size: 24px;
    width: 60px;
    text-align: center;
  }

  .custom-dice-input {
    display: flex;
    align-items: center;
    font-size: 24px;

    .d-prefix {
      margin-right: 2px;
    }

    .custom-sides-input {
      min-width: 40px;
      width: 43px;
      font-size: 24px;
      text-align: center;
      border: none;
      border-radius: 5px;
      outline: none;
      box-sizing: border-box;
      background-color: var(--input-color);
      color: var(--text-color);
      transition: 0.2s all;

      &::-webkit-outer-spin-button,
      &::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
      }
      &:hover {
        background-color: var(--input-hover-color);
      }
      &:focus {
        background-color: var(--input-focus-color);
      }
    }
  }
}

.roll-button {
  margin-top: 10px;
  cursor: pointer;
}
</style>
