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
@import url('https://fonts.googleapis.com/css2?family=Inknut+Antiqua:wght@300;400;500;600;700;800;900&family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap');

:root {
    --background-color: #e9e9e9;
    --text-color: #383838;
    --header-text-color: #a4a4a4;
    --header-text-hover-color: #707070;
    --button-hover-background: #e0e0e0;
    --button-active-background: #cecece;
    --input-color: #d8d8d8;
    --input-hover-color: #d6d6d6;
    --input-focus-color: #cccccc;
  }
  
  .dark {
    --background-color: #232323;
    --text-color: #ffffff;
    --header-text-color: #9a9a9a;
    --header-text-hover-color: #e8e8e8;
    --button-hover-background: #343434;
    --button-active-background: #464646;
    --input-color: #303030;
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
