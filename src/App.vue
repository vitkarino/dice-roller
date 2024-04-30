<template>
  <HeaderComponent />
  <div class="main-container">
    <DiceComponent />
    <RollResultComponent />
  </div>
</template>

<script>
import HeaderComponent from "./components/HeaderComponent.vue";
import DiceComponent from "./components/DiceComponent.vue";
import RollResultComponent from "./components/RollResultComponent.vue";

export default {
  components: { HeaderComponent, DiceComponent, RollResultComponent },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: "Inter", sans-serif;
  overflow: hidden;
}

.main-container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  height: 90vh;
}

button {
  width: 200px;
  height: 40px;
  background: none;
  border-radius: 25px;
  border: 2px solid black;
  transition: transform 0.2s;

  .pop {
    transform: scale(1.2); /* Enlarge the button */
  }

  &:hover {
    background-color: rgba(0, 0, 0, 0.1);
    transition: 0.2s all;
  }
}
</style>
