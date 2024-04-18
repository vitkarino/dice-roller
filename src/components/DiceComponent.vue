<template>
  <div class="dice-roller">
    <Icon :icon="currentDice" class="dice-icon" :class="{ animateDice: isAnimating }" />
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
  <button class="roll-button">Roll</button>
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

    const diceNames = ["d4", "d6", "d8", "d10", "d12", "d20"];

    const isAnimating = ref(false);
    const currentDiceIndex = ref(0);
    const diceName = computed(() => diceNames[currentDiceIndex.value]);

    const animateDice = () => {
      isAnimating.value = true;
      setTimeout(() => (isAnimating.value = false), 300);
    };

    const nextDice = async () => {
      if (isAnimating.value) return;
      if (currentDiceIndex.value < 5) {
        animateDice();
        await new Promise((resolve) => setTimeout(resolve, 200));
        currentDiceIndex.value = (currentDiceIndex.value + 1) % diceList.length;
      }
    };

    const previousDice = async () => {
      if (isAnimating.value) return;
      if (currentDiceIndex.value > 0) {
        animateDice();
        await new Promise((resolve) => setTimeout(resolve, 200));
        currentDiceIndex.value =
          (currentDiceIndex.value - 1 + diceList.length) % diceList.length;
      }
    };

    const currentDice = computed(() => diceList[currentDiceIndex.value]);

    return {
      currentDice,
      nextDice,
      previousDice,
      isAnimating,
      animateDice,
      diceName,
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
  }

  .animateDice {
    animation: dice-animation 0.5s;
  }

  @keyframes dice-animation {
    0% {
      transform: translateY(-10%);
      opacity: 1;
    }

    50% {
      transform: translateY(10%);
      opacity: 0;
    }
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
