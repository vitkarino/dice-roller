<template>
  <div class="dice-roller">
    <div class="dice" @click="rollDice()" :class="{
      changeDiceAnimation: isChangingDice,
      rollAnimation: isRolling,
    }">
      <img class="dice-icon" :src="`/${diceName}.svg`" alt="Dice icon"/>
      <span class="roll-result" :class="{ lowerPosition: isWrongSizing }">{{ rollResult }}</span>
      </img>
    </div>
  </div>
  <div class="dice-selection">
    <button class="select-button" id="left" @click="previousDice" :disabled="diceName === 'd4'">
      <Icon icon="ep:arrow-up" rotate="270deg" />
    </button>
    <span class="dice-name">{{ diceName }}</span>
    <button class="select-button" id="right" @click="nextDice" :disabled="diceName === 'd100'">
      <Icon icon="ep:arrow-up" rotate="90deg" />
    </button>
  </div>
  <button class="roll-button" @click="rollDice">Roll</button>
</template>

<script>
import { ref, computed, onMounted } from "vue";

export default {
  setup() {
    const diceNames = ["d4", "d6", "d8", "d10", "d12", "d20", "d100"];
    const rollResult = ref(1);
    const isChangingDice = ref(false);
    const isRolling = ref(false);
    const currentDiceIndex = ref(0);
    const diceName = computed(() => `${diceNames[currentDiceIndex.value]}`);

    const isWrongSizing = computed(() => ['d12', 'd4'].includes(diceName.value));

    onMounted(() => {
      diceNames.forEach((dice) => {
        const img = new Image();
        img.src = `/${dice}.svg`;
      })
    })

    //Function to detect which animation should be running
    const toggleAnimation = (type, duration) => {
      if (type === 'roll') {
        isRolling.value = true;
        setTimeout(() => isRolling.value = false, duration);
      } else {
        isChangingDice.value = true;
        setTimeout(() => isChangingDice.value = false, duration);
      }
    };

    //Function to change the dice (next or previous)
    const changeDice = (direction) => {
      if (isChangingDice.value || isRolling.value) return;
      if (direction === 1 && currentDiceIndex.value === diceNames.length - 1) return; 
      if (direction === -1 && currentDiceIndex.value === 0) return; 

      toggleAnimation('change', 165);
      currentDiceIndex.value = (currentDiceIndex.value + direction + diceNames.length) % diceNames.length;
      rollResult.value = 1;
    };

    //Function to roll selected dice
    const rollDice = () => {
      if (isChangingDice.value || isRolling.value) return;
      toggleAnimation('roll', 500)
      setTimeout(() => {
        rollResult.value = Math.floor(Math.random() * parseInt(diceNames[currentDiceIndex.value].slice(1)) + 1);
      }, 150);
    };

    return { diceName, rollResult, rollDice, nextDice: () => changeDice(1), previousDice: () => changeDice(-1), isWrongSizing, isChangingDice, isRolling };
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
}

.roll-button {
  margin-top: 10px;
  cursor: pointer;
}
</style>
