<!-- components/ScratchCard.vue -->
<template>
  <div class="scratch-card" ref="scratchCardContainer">
    <div class="flex w-full h-full justify-center items-center">
      <p
        style="
          font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
          color: seashell;
          font-size: 2rem;
          text-align: center;

          background: transparent;
        "
      >
        Happiest BirthDay Akka 🎉💚
      </p>
    </div>

    <canvas ref="scratchCanvas" class="scratch-layer"></canvas>
    <div class="hidden-content" :class="{ show: isScratched }">
      <slot></slot>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";

const scratchCardContainer = ref(null);
const scratchCanvas = ref(null);
let isScratched = false;

onMounted(() => {
  const canvas = scratchCanvas.value;
  const context = canvas.getContext("2d");
  const container = scratchCardContainer.value;

  const width = container.offsetWidth;
  const height = container.offsetHeight;

  canvas.width = width;
  canvas.height = height;

  // Draw the scratchable overlay
  context.fillStyle = "#888"; // Scratch overlay color
  context.fillRect(0, 0, width, height);

  // Add text on the canvas
  context.fillStyle = "#333"; // Text color
  context.font = "24px Arial"; // Font size and family
  context.textAlign = "center";
  context.fillText("Scratch here!", width / 2, height / 2); // Text position

  // Event listeners for mouse and touch events
  canvas.addEventListener("mousedown", startScratch);
  canvas.addEventListener("mouseup", stopScratch);
  canvas.addEventListener("mousemove", scratch);
  canvas.addEventListener("touchstart", startScratch);
  canvas.addEventListener("touchend", stopScratch);
  canvas.addEventListener("touchmove", scratch);

  // Function to handle starting scratch
  function startScratch(e) {
    e.preventDefault();
    isScratched = true;
  }

  // Function to handle stopping scratch
  function stopScratch() {
    isScratched = false;
  }

  // Function to handle scratching
  function scratch(e) {
    e.preventDefault();
    if (isScratched) {
      const rect = canvas.getBoundingClientRect();
      const x = e.touches
        ? e.touches[0].clientX - rect.left
        : e.clientX - rect.left;
      const y = e.touches
        ? e.touches[0].clientY - rect.top
        : e.clientY - rect.top;

      context.globalCompositeOperation = "destination-out";
      context.beginPath();
      context.arc(x, y, 20, 0, Math.PI * 2);
      context.fill();
    }
  }
});
</script>

<style scoped>
.scratch-card {
  position: relative;
  width: 300px; /* Adjust as needed */
  height: 200px; /* Adjust as needed */
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  background: linear-gradient(
    135deg,
    #75b8f2,
    #5069e4,
    #68ecee,
    #a55cea,
    #ce36e8
  );
}

.scratch-layer {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  cursor: pointer;
}

.hidden-content {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: none; /* Initially hidden */
  justify-content: center;
  align-items: center;
  color: #333;
  font-size: 1.5rem;
  font-weight: bold;
  background: #fff;
  border-radius: 8px;
}

.hidden-content.show {
  display: flex; /* Show when isScratched is true */
}
</style>
