<template>
  <button class="fancy-button">
    <span class="button-wrapper">
      <span>{{ text }}</span>
      <span>{{ text }}</span>
    </span>
    {{ text }}
  </button>
</template>

<script>
export default {
  data() {
    return {
      text: "menu",
    };
  },
  computed: {
    formatText() {
      return this.text.split("");
    },
  },
};
</script>

<style scoped>
button {
  background: none;
  border: none;
  cursor: pointer;
  text-align: center;
  font-size: 14px;
  font-weight: 400;
  letter-spacing: 0.24em;
}

.fancy-button {
  color: rgba(0, 0, 0, 0);
  position: relative;
  overflow: hidden;
}

.fancy-button .button-wrapper {
  display: flex;
  flex-direction: column;
  color: rgba(0, 0, 0, 1);
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  text-transform: uppercase;
  transform: translateX(0%);
  transition: transform 0.2s ease-out;
}

.button-wrapper span {
  height: 100%;
  display: flex;
  text-align: center;
  justify-content: center;
}

.fancy-button:hover .button-wrapper {
  transform: translateY(-50%);
}
</style>