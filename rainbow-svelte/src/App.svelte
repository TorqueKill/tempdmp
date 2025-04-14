<script>
  const NUM_IMAGES = 30;
  let clickCount = 0;
  let showJumpscare = false;

  let images = Array.from({ length: NUM_IMAGES }, () => ({
    x: Math.random() * window.innerWidth,
    y: Math.random() * window.innerHeight,
    z: Math.random() * -1000 - 200,
    size: Math.random() * 150 + 150,
    angle: Math.random() * 360,
    speed: Math.random() * 0.5 + 0.2,
    rotationSpeed: Math.random() * 2 - 1
  }));

  const animate = () => {
    images = images.map(img => {
      let newZ = img.z + img.speed * 5;

      if (newZ > 300) {
        return {
          ...img,
          x: Math.random() * window.innerWidth,
          y: Math.random() * window.innerHeight,
          z: -1000,
          size: Math.random() * 150 + 220,
          speed: Math.random() * 0.5 + 0.2,
          rotationSpeed: Math.random() * 2 - 1,
          angle: Math.random() * 360
        };
      }

      return {
        ...img,
        z: newZ,
        angle: img.angle + img.rotationSpeed
      };
    });

    requestAnimationFrame(animate);
  };

  animate();

  const handleClick = () => {
    clickCount += 1;
    if (clickCount === 10) {
      showJumpscare = true;
      setTimeout(() => {
        showJumpscare = false;
        clickCount = 0; // Reset counter so it can happen again
      }, 2000);
    }
  };
</script>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
    overflow: hidden;
    background: linear-gradient(
      45deg,
      red,
      orange,
      yellow,
      green,
      blue,
      indigo,
      violet
    );
    background-size: 400% 400%;
    animation: bgShift 10s ease infinite;
    height: 100vh;
    width: 100vw;
  }

  @keyframes bgShift {
    0% {
      background-position: 0% 50%;
    }
    50% {
      background-position: 100% 50%;
    }
    100% {
      background-position: 0% 50%;
    }
  }

  .scene {
    position: absolute;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    perspective: 1000px;
    perspective-origin: left center;
    overflow: hidden;
  }

  .floating-img {
    position: absolute;
    transform-style: preserve-3d;
    will-change: transform;
  }

  .jumpscare {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: black;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 9999;
    animation: jumpscareIn 0.4s ease-out forwards;
    transform: scale(3); /* Start at 3x size */
    user-select: none; /* Prevent the image from being selected */
  }

  .jumpscare img {
    max-width: 300%;
    max-height: 300%;
  }

  @keyframes jumpscareIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }

  @keyframes pulseZoom {
    0% {
      transform: scale(0.2) rotate(0deg);
    }
    20% {
      transform: scale(1.1) rotate(2deg);
    }
    50% {
      transform: scale(1.3) rotate(-2deg);
    }
    100% {
      transform: scale(1) rotate(0deg);
    }
  }
</style>

<div class="scene" on:click={handleClick}>
  {#each images as img (img)}
    <img
      class="floating-img"
      src="/bdayboi.png"
      alt="space boi"
      style="
        transform:
          translate3d({img.x}px, {img.y}px, {img.z}px)
          rotateY({img.angle}deg);
        width: {img.size}px;
      "
    />
  {/each}

  {#if showJumpscare}
    <div class="jumpscare">
      <img src="/jumpscare.png" alt="BOO!" />
    </div>
  {/if}
</div>
