<template>
  <HeaderComponent />
  <div class="main-container">
    <DiceComponent />
  </div>
</template>

<script>
import HeaderComponent from "./components/HeaderComponent.vue";
import DiceComponent from "./components/DiceComponent.vue";

export default {
  components: { HeaderComponent, DiceComponent },
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
  transition: 0.2s;
  -webkit-tap-highlight-color: transparent;
  -webkit-touch-callout: none; 
  -webkit-user-select: none; 
  -khtml-user-select: none; 
  -moz-user-select: none; 
  -ms-user-select: none; 
  user-select: none; 

  .pop {
    transform: scale(1.2);
  }

  @media (min-width: 768px) {
    &:hover {
      background-color: rgba(0, 0, 0, 0.05);
      transition: 0.2s all;
    }
  }

  &:active {
    background-color: rgba(0, 0, 0, 0.1);
  }
}
</style>
