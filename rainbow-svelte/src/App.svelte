<script>
  const NUM_IMAGES = 20;

  let images = Array.from({ length: NUM_IMAGES }, () => ({
    x: Math.random() * window.innerWidth,
    y: Math.random() * window.innerHeight,
    z: Math.random() * -1000 - 200,
    size: Math.random() * 150 + 120,
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
          size: Math.random() * 150 + 50,
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
    left: -500px;
    width: 200vw;
    height: 100vh;
    perspective: 1000px;
    perspective-origin: left center; /* 💥 key change */
    overflow: hidden;
  }

  .floating-img {
    position: absolute;
    transform-style: preserve-3d;
    will-change: transform;
  }
</style>

<div class="scene">
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
</div>
