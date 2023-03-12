<script lang="ts">
  import { spring } from "svelte/motion";

  let x = spring(0);
  let y = spring(0);

  function handleMouseMove(event: MouseEvent) {
    x.set(event.clientX - 200);
    y.set(event.clientY - 200);
  }
</script>

<div id="container" on:mousemove={handleMouseMove}>
  <div id="home">
    <div id="portal" style="top: {$y}px; left: {$x}px">
      <div id="away" />
    </div>
  </div>
</div>

<style>
  #container {
    position: absolute;
    top: 0;
    left: 0;
  }
  #home {
    background-image: url("/alex-a.jpeg");
    background-size: cover;
    width: 100vw;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    clip: rect(0, auto, auto, 0);
  }
  #away {
    background-image: url("/alex.jpg");
    background-size: cover;
    background-attachment: fixed;
    width: 100vw;
    height: 100vh;
  }
  #portal {
    position: fixed;
    top: 45vh;
    left: 50vw;
    height: 400px;
    width: 400px;
    border-radius: 300px;
    clip: rect(0, auto, auto, 0);
  }
</style>
