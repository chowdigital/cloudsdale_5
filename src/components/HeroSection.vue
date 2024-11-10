<template>
  <section class="hero-section">
    <div class="hero-content">
      <h1>{{ message }}</h1>
      <p>Your amazing journey begins here.</p>
      <button @click="changeMessage">Click me!</button>
    </div>
    <!-- Div for the mouse-following effect -->
    <div class="mouse-effect"></div>
  </section>
</template>

<script>
export default {
  name: "HeroSection",
  data() {
    return {
      message: "Welcome to My Website",
      targetX: 0, // The target position of the cursor
      targetY: 0,
      currentX: 0, // The current position of the circle
      currentY: 0,
    };
  },
  methods: {
    changeMessage() {
      this.message = "You clicked the button!";
    },
    handleMouseMove(event) {
      // Update targetX and targetY with the cursor position
      this.targetX = event.clientX - 250; // Center circle horizontally
      this.targetY = event.clientY - 250; // Center circle vertically
    },
    animateCircle() {
      // Smoothly update currentX and currentY towards targetX and targetY
      this.currentX += (this.targetX - this.currentX) * 0.15; // Adjust the 0.15 factor for more or less delay
      this.currentY += (this.targetY - this.currentY) * 0.15;

      // Apply the transform to the .mouse-effect div
      const mouseEffect = this.$el.querySelector(".mouse-effect");
      mouseEffect.style.transform = `translate(${this.currentX}px, ${this.currentY}px)`;

      // Call animateCircle again on the next animation frame
      requestAnimationFrame(this.animateCircle);
    },
  },
  mounted() {
    // Start the animation loop and set up the mousemove listener
    window.addEventListener("mousemove", this.handleMouseMove);
    this.animateCircle();
  },
  beforeUnmount() {
    // Clean up the event listener when component is destroyed
    window.removeEventListener("mousemove", this.handleMouseMove);
  },
};
</script>

<style scoped lang="scss">
.hero-section {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: white;
  overflow: hidden;
  background: 
      /* Fixed pink to yellow gradient */ linear-gradient(
      120deg,
      #f3dbd5,
      transparent 30%,
      transparent 70%,
      #f4dc6d
    ),
    radial-gradient(at center, #d1e0de, #75aee1);
  background-size: 100% 100%, 200% 200%;
}

.mouse-effect {
  position: fixed;
  top: 0;
  left: 0;
  width: 500px; /* Diameter of the circle */
  height: 500px;
  pointer-events: none; /* Ensures it doesn't interfere with clicks */
  border-radius: 50%;
  background-color: rgba(255, 255, 255, 0.5); /* Semi-transparent white */
  filter: blur(100px); /* Soft blur effect */
  transition: transform 0.1s ease-out; /* Smooth movement */
}
</style>
