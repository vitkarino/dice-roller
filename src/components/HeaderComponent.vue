<template>
  <header>
    <p>Dice Roller by vitkarino - 2024</p>
    <div class="icons-container">
      <span class="icon">
        <a href="https://www.instagram.com/vitkarino">
          <Icon icon="mdi:instagram" class="social-icon" />
        </a>
      </span>
      <span class="icon">
        <a href="https://github.com/vitkarino">
          <Icon icon="mdi:github" class="social-icon" />
        </a>
      </span>
    </div>
  </header>
</template>

<script>
import { Icon } from "@iconify/vue";

export default {
  components: {
    Icon,
  },
};
</script>

<style scoped lang="scss">
header {
  height: 10vh;
  width: 100%;
  display: flex;
  position: fixed;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  color: rgba($color: #000000, $alpha: 0.3);

  p {
    font-size: 20px;
    margin-bottom: 10px;
  }

  .icon {
    font-size: 24px;
    margin: 0 5px 0 5px;
    transition: 0.1s all;
    cursor: pointer;

    a {
      color: rgba($color: #000000, $alpha: 0.2);
      transition: 0.2s;
      &:hover {
        color: rgba($color: #000000, $alpha: 1);
        transition: 0.2s;
      }
    }
  }
}
</style>
