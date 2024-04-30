<template>
  <div class="result-container">
    <p class="result">20</p>
    <p>Roll result</p>
  </div>
</template>

<style scoped lang="scss">
.result-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: fixed;
  bottom: 30px;
  border-radius: 25px;
  border: 2px solid black;
  width: 300px;
  height: 100px;

  .result {
    font-size: 42px;
  }
}
</style>
