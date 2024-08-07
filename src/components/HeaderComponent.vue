<template>
  <header>
    <span> <span class="logo">QuickDice</span> | By Viktor Kysil </span>
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
      <span class="icon">
        <a @click="toggleTheme()">
          <Icon
            :icon="theme === 'light' ? 'ph:moon' : 'ph:sun'"
            class="social-icon"
          />
        </a>
      </span>
    </div>
  </header>
</template>

<script setup>
import { ref, onMounted } from "vue";

const theme = ref("light");

onMounted(() => {
  const savedTheme = localStorage.getItem("theme");
  if (savedTheme) {
    theme.value = savedTheme;
    document.documentElement.classList.add(theme.value);
  }
});

function toggleTheme() {
  theme.value = theme.value === "light" ? "dark" : "light";
  document.documentElement.classList.toggle("dark", theme.value === "dark");
  console.log(theme.value);
  localStorage.setItem("theme", theme.value);
}
</script>

<style scoped lang="scss">
header {
  height: 12vh;
  width: 100%;
  display: flex;
  position: fixed;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  color: var(--header-text-color);

  span {
    font-size: 18px;
    -webkit-tap-highlight-color: transparent;

    .logo {
      font-weight: 600;
      font-family: "Inknut Antiqua", sans-serif;
    }
  }

  .icon {
    font-size: 24px;
    margin: 0 5px;
    transition: 0.1s all;
    cursor: pointer;

    a {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      color: var(--header-text-color);
      transition: 0.2s;
      user-select: none;
      outline: none;

      &:hover {
        color: var(--header-text-hover-color);
      }
    }
  }
}
</style>
