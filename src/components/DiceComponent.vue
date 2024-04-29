<template>
  <div class="dice-roller">
    <Icon
      :icon="currentDice"
      class="dice-icon"
      :class="{ changeDiceAnimation: isChangingDice, rollAnimation: isRolling }"
    />
  </div>
  <div class="dice-selection">
    <button class="select-button" id="left" @click="previousDice()">
      <Icon icon="ep:arrow-up" rotate="270deg" />
    </button>
    <span class="dice-name">{{ diceName }}</span>
    <button class="select-button" id="right" @click="nextDice()">
      <Icon icon="ep:arrow-up" rotate="90deg" />
    </button>
  </div>
  <button class="roll-button" @click="rollDice()">Roll</button>
</template>

<script>
import { ref, computed } from "vue";
import { Icon } from "@iconify/vue";

export default {
  components: { Icon },
  setup() {
    const diceList = [
      "mdi:dice-d4-outline",
      "mdi:dice-d6-outline",
      "mdi:dice-d8-outline",
      "mdi:dice-d10-outline",
      "mdi:dice-d12-outline",
      "mdi:dice-d20-outline",
    ];

    const diceNames = [
      {
        Name: "d4",
        Roll: 4,
      },
      {
        Name: "d6",
        Roll: 6,
      },
      {
        Name: "d8",
        Roll: 8,
      },
      {
        Name: "d10",
        Roll: 10,
      },
      {
        Name: "d12",
        Roll: 12,
      },
      {
        Name: "d20",
        Roll: 20,
      },
    ];
    var rollResult;

    const isChangingDice = ref(false);
    const isRolling = ref(false);
    const currentDiceIndex = ref(0);
    const diceName = computed(() => diceNames[currentDiceIndex.value].Name);

    const animateRoll = () => {
      isRolling.value = true;
      setTimeout(() => (isRolling.value = false), 500); // match the duration of roll animations
    };

    const animateDiceChange = () => {
      isChangingDice.value = true;
      setTimeout(() => (isChangingDice.value = false), 200); // match the duration of dice change animations
    };

    const nextDice = async () => {
      if (isChangingDice.value || isRolling.value) return;
      if (currentDiceIndex.value < 5) {
        animateDiceChange();
        await new Promise((resolve) => setTimeout(resolve, 100));
        currentDiceIndex.value =
          (currentDiceIndex.value + 1) % diceNames.length;
      }
    };

    const previousDice = async () => {
      if (isChangingDice.value || isRolling.value) return;
      if (currentDiceIndex.value > 0) {
        animateDiceChange();
      await new Promise((resolve) => setTimeout(resolve, 100));
      currentDiceIndex.value =
        (currentDiceIndex.value - 1 + diceNames.length) % diceNames.length;
      }
    };

    const rollDice = () => {
      if (isChangingDice.value || isRolling.value) return;
      animateRoll();
      setTimeout(() => {
        rollResult = Math.floor(
          Math.random() * diceNames[currentDiceIndex.value].Roll + 1
        );
        console.log(
          `Rolled ${
            diceNames[currentDiceIndex.value].Name
          }, Result: ${rollResult}`
        );
      }, 300);
    };

    const currentDice = computed(() => diceList[currentDiceIndex.value]);

    return {
      currentDice,
      nextDice,
      previousDice,
      isChangingDice,
      isRolling,
      animateRoll,
      animateDiceChange,
      diceName,
      rollDice,
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
  margin: 10px 0 10px 0;

  .dice-icon {
    font-size: 150px;

    &.rollAnimation {
      animation: roll 0.5s ease-in-out;
    }
    &.changeDiceAnimation {
      animation: dice-animation 0.2s ease-in-out;
    }
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

@keyframes roll {
  0% {
    transform: rotate(0deg) scale(0.8);
  }
  100% {
    transform: rotate(360deg) scale(1);
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
    transition: 0.1s all;
    cursor: pointer;
  }

  .dice-name {
    font-size: 24px;
    width: 50px;
    text-align: center;
  }
}

.roll-button {
  margin-top: 10px;
  transition: 0.1s all;
  cursor: pointer;
}
</style>
