<template>
  <div class="dice-roller">
    <div
      class="dice"
      @click="rollDice()"
      :class="{
        changeDiceAnimation: isChangingDice,
        rollAnimation: isRolling,
      }"
    >
      <img class="dice-icon" :src="`/${diceName}.svg`" alt="Dice icon" />
      <span class="roll-result" :class="{ lowerPosition: isWrongSizing }">{{
        rollResult
      }}</span>
    </div>
  </div>
  <div class="dice-selection">
    <button
      class="select-button"
      id="left"
      @click="previousDice"
      :disabled="diceName === 'd4'"
    >
      <Icon class="arrow-left" icon="ep:arrow-up" rotate="270deg" />
    </button>
    <span class="dice-name" v-if="diceName !== 'd'">{{ diceName }}</span>
    <div v-else class="custom-dice-input">
      <span class="d-prefix">d</span>
      <input
        v-model="customSides"
        type="number"
        min="2"
        max="1000"
        class="custom-sides-input"
        @input="handleCustomSidesInput"
        ref="customInput"
      />
    </div>
    <button
      class="select-button"
      id="right"
      @click="nextDice"
      :disabled="diceName === 'd'"
    >
      <Icon class="arrow-right" icon="ep:arrow-up" rotate="90deg" />
    </button>
  </div>
  <button class="roll-button" @click="rollDice">Roll</button>
</template>


<script>
import { ref, computed, onMounted, watch } from "vue";

export default {
  setup() {
    const diceNames = ["d4", "d6", "d8", "d10", "d12", "d20", "d100", "d"];
    const rollResult = ref(1);
    const isChangingDice = ref(false);
    const isRolling = ref(false);
    const currentDiceIndex = ref(0);
    const customSides = ref(2);
    const customInput = ref(null);
    const diceName = computed(() => `${diceNames[currentDiceIndex.value]}`);

    const isWrongSizing = computed(() => ['d12', 'd4'].includes(diceName.value));

    onMounted(() => {
      diceNames.forEach((dice) => {
        const img = new Image();
        img.src = `/${dice}.svg`;
      });
      resizeInput();
    });

    watch(customSides, resizeInput);

    function resizeInput() {
      if (customInput.value) {
        const minWidth = 40;
        const inputWidth = Math.max(minWidth, customSides.value.toString().length * 15);
        customInput.value.style.width = `${inputWidth}px`;
      }
    }

    const toggleAnimation = (type, duration) => {
      if (type === 'roll') {
        isRolling.value = true;
        setTimeout(() => isRolling.value = false, duration);
      } else {
        isChangingDice.value = true;
        setTimeout(() => isChangingDice.value = false, duration);
      }
    };

    const changeDice = (direction) => {
      if (isChangingDice.value || isRolling.value) return;
      if (direction === 1 && currentDiceIndex.value === diceNames.length - 1) return; 
      if (direction === -1 && currentDiceIndex.value === 0) return; 

      toggleAnimation('change', 165);
      currentDiceIndex.value = (currentDiceIndex.value + direction + diceNames.length) % diceNames.length;
      rollResult.value = 1;
    };

    const rollDice = () => {
      if (isChangingDice.value || isRolling.value) return;
      toggleAnimation('roll', 500)
      setTimeout(() => {
        if (diceName.value === 'd') {
          rollResult.value = Math.floor(Math.random() * customSides.value + 1);
        } else {
          rollResult.value = Math.floor(Math.random() * parseInt(diceNames[currentDiceIndex.value].slice(1)) + 1);
        }
      }, 150);
    };

    const handleCustomSidesInput = () => {
      if (customSides.value < 2) customSides.value = 2;
      if (customSides.value > 1000) customSides.value = 1000;
    };

    return { 
      diceName, 
      rollResult, 
      rollDice, 
      nextDice: () => changeDice(1), 
      previousDice: () => changeDice(-1), 
      isWrongSizing, 
      isChangingDice, 
      isRolling,
      customSides,
      customInput,
      resizeInput,
      handleCustomSidesInput
    };
  },
};
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

    &.rollAnimation {
      animation: roll 0.5s ease-in-out;
    }

    &.changeDiceAnimation {
      animation: dice-animation 0.2s ease-in-out;
    }

    .dice-icon {
      width: 150px;
    }

    .roll-result {
      position: absolute;
      font-size: 42px;
      font-weight: 800;

      &.lowerPosition {
        transform: translateY(7px);
      }
    }
  }
}

@keyframes roll {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
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
      background-color: rgba(0, 0, 0, 0.03);
      transition: 0.2s all;
      -moz-appearance: textfield;
      &::-webkit-outer-spin-button,
      &::-webkit-inner-spin-button {
        -webkit-appearance: none;
        margin: 0;
      }
      &:hover {
        background-color: rgba(0, 0, 0, 0.06);
      }
      &:focus {
        background-color: rgba(0, 0, 0, 0.08);
      }
    }
  }
}

.roll-button {
  margin-top: 10px;
  cursor: pointer;
}
</style>
