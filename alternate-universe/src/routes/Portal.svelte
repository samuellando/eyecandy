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
    <div id="homeimg" style="top: {$y}px; left: {$x}px" />
  </div>
  <div id="away">
    <div id="awayimg" style="top: {$y}px; left: {$x}px" />
  </div>
</div>

<style>
  #container {
    position: absolute;
    top: 0;
    left: 0;
  }
  #home {
    background-image: url("/bg.jpeg");
    background-size: cover;
    width: 100vw;
    height: 50vh;
    position: absolute;
    top: 0;
    left: 0;
    clip: rect(0, auto, auto, 0);
  }
  #away {
    background-image: url("/vg-bg.jpg");
    background-size: cover;
    background-color: green;
    width: 100vw;
    height: 50vh;
    overflow: hidden;
    position: absolute;
    top: 50vh;
    left: 0;
    clip: rect(0, auto, auto, 0);
  }
  #homeimg {
    position: fixed;
    top: 45vh;
    left: 50vw;
    height: 400px;
    width: 400px;
    background-image: url("/horse.png");
    background-size: cover;
  }
  #awayimg {
    position: fixed;
    top: 45vh;
    left: 50vw;
    height: 400px;
    width: 400px;
    background-color: yellow;
    background-image: url("/vg-horse.png");
    background-size: cover;
  }
</style>
