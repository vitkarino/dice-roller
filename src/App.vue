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

:root {
    --background-color: #f4f4f4;
    --text-color: #383838;
    --header-text-color: #c7c7c7;
    --header-text-hover-color: #878787;
    --button-hover-background: #e8e8e8;
    --button-active-background: #e6e6e6;
    --input-color: #f1f1f1;
    --input-hover-color: #e7e7e7;
    --input-focus-color: #dbdbdb;
  }
  
  .dark {
    --background-color: #232323;
    --text-color: #e3e3e3;
    --header-text-color: #7b7b7b;
    --header-text-hover-color: #e8e8e8;
    --button-hover-background: #343434;
    --button-active-background: #464646;
    --input-color: #2d2d2d;
    --input-hover-color: #383838;
    --input-focus-color: #454545;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: "Inter", sans-serif;
  overflow: hidden;
}

html {
  background-color: var(--background-color);
  transition: .1s all
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
  border: 2px solid var(--text-color);
  color: var(--text-color);
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
      background-color: var(--button-hover-background);
      transition: 0.2s all;
    }
  }

  &:active {
    background-color: var(--button-active-background);
  }
}
</style>
