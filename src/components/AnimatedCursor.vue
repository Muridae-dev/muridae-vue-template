<template>
  <div :style="{ position: 'fixed', top: '0', left: '0', zIndex: '1000' }">
    <div class="cursor-box" ref="cursorBox" id="cursorBox"></div>
    <div class="cursor-dot" ref="cursorDot">
      <div class="inner-cursor-box" ref="innerBox"></div>
      <div class="inner-cursor-box2" ref="innerBox2"></div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from "vue";

export default defineComponent({
  setup() {
    const cursorBox = ref<HTMLDivElement | null>(null);
    const cursorBoxData = ref({ x: 0, y: 0 });
    const cursorDot = ref<HTMLDivElement | null>(null);
    const cursorDotData = ref({ x: 0, y: 0 });
    const innerBox = ref(null);
    const innerBox2 = ref(null);
    const mouseX = ref(0);
    const mouseY = ref(0);

    const lerp = (start, end, amt) => {
      return (1 - amt) * start + amt * end;
    };

    const updateCursorBox = () => {
      if (cursorBox.value) {
        cursorBox.value.style.transform = `translate3d(calc(${cursorBoxData.value.x}px - 50%), calc(${cursorBoxData.value.y}px - 50%), 0)`;
      }
    };

    const updateCursorDot = () => {
      if (cursorDot.value) {
        cursorDot.value.style.transform = `translate3d(calc(${cursorDotData.value.x}px - 50%), calc(${cursorDotData.value.y}px - 50%), 0)`;
      }
    };

    const followMouse = (e) => {
      mouseX.value = e.pageX;
      mouseY.value = e.pageY;
    };

    const clickMouse = () => {
      innerBox.value.style.animation = "";
      innerBox2.value.style.animation = "";
      setTimeout(() => {
        innerBox.value.style.animation = "mouseClick 1s";
        innerBox2.value.style.animation = "mouseClick2 1s";
      }, 10);
    };

    const move = () => {
      cursorDotData.value.x = lerp(cursorDotData.value.x, mouseX.value, 0.7);
      cursorDotData.value.y = lerp(cursorDotData.value.y, mouseY.value, 0.7);
      updateCursorDot();

      cursorBoxData.value.x = lerp(cursorBoxData.value.x, mouseX.value, 0.1);
      cursorBoxData.value.y = lerp(cursorBoxData.value.y, mouseY.value, 0.1);
      updateCursorBox();

      requestAnimationFrame(move);
    };

    onMounted(() => {
      document.addEventListener("mousemove", followMouse, false);
      document.addEventListener("mousedown", clickMouse, false);
      move();
    });

    return {
      cursorBox,
      cursorDot,
      innerBox,
      innerBox2,
      mouseX,
      mouseY,
    };
  },
});
</script>

<style lang="scss">
.cursor-box {
  height: 50px;
  width: 50px;
  position: absolute;
  border-radius: 50%;
  border: 3px solid white;
  pointer-events: none;
  mix-blend-mode: difference;
  z-index: 99;

  @media (max-width: 768px) {
    border: none;
  }
}

.cursor-dot {
  width: 20px;
  height: 20px;
  background-color: white;
  opacity: 0.8;
  border-radius: 50%;
  position: absolute;
  transform: translate(-50%, -50%);
  mix-blend-mode: difference;
  z-index: 99;
  pointer-events: none;

  @media (max-width: 768px) {
    background-color: transparent;
  }

  .inner-cursor-box,
  .inner-cursor-box2 {
    width: 0;
    height: 0;
    background-color: white;
    opacity: 0.6;
    border-radius: 50%;
    position: absolute;
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
  }

  .inner-cursor-box2 {
    opacity: 0.8;
    background-color: transparent;
  }
}

@keyframes mouseClick {
  from {
    width: 0;
    height: 0;
    opacity: 0.6;
  }

  to {
    width: 600%;
    height: 600%;
    opacity: 0;
  }
}

@keyframes mouseClick2 {
  from {
    width: 0;
    height: 0;
    opacity: 0.8;
    border: 3px solid white;
  }

  to {
    width: 800%;
    height: 800%;
    opacity: 0;
    border: 3px solid white;
  }
}
</style>
