<template>
  <div id="tela" class="home" @click="handleClick">
    <div
      v-for="item in dots"
      :key="item.id"
      class="home__dot"
      :style="{ left: item.positionX, top: item.positionY }"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";

let dots = ref([]);
let dotsRedo = ref([]);

const handleClick = (event) => {
  const newDot = {
    id: dots.value.length,
    positionX: event.pageX - 4 + "px",
    positionY: event.pageY - 4 + "px",
  };

  dots.value = [...dots.value, newDot];
  dotsRedo.value = [];
};

const undo = () => {
  if (!dots.value.length) return;

  dotsRedo.value = [...dotsRedo.value, dots.value[dots.value.length - 1]];
  dots.value.pop();
};

const redo = () => {
  if (!dotsRedo.value.length) return;

  dots.value = [...dots.value, dotsRedo.value[dotsRedo.value.length - 1]];
  dotsRedo.value.pop();
};

onMounted(() => {
  document.addEventListener("keyup", keyupHandler);
});

const keyupHandler = (event) => {
  if (event.ctrlKey && event.key === "z") {
    undo();
  }

  if (event.ctrlKey && event.key === "y") {
    redo();
  }
};
</script>

<style scoped lang="scss">
.home {
  width: 100%;
  height: 100vh;

  &__dot {
    width: 8px;
    height: 8px;
    background-color: blue;
    position: absolute;
    border-radius: 50%;
  }
}
</style>
