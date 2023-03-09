<script lang="ts">
  import { spring } from "svelte/motion";
  import AniChar from "./AniChar.svelte";
  import type { Spring } from "svelte/motion";
  import { onMount, tick } from "svelte";

  export let text: string;

  interface point {
    x: number;
    y: number;
  }

  interface char {
    id: number;
    char: string;
    x: number;
    y: number;
    w: Spring<number>;
  }

  let id = 0;
  let spans: char[] = text.split("").map((c) => {
    return {
      id: id++,
      char: c,
      x: 0,
      y: 0,
      w: spring(100),
    };
  });

  let curX = 0;
  let curY = 0;

  onMount(() => {
    var domspans = document.getElementsByTagName("span");
    id = 0;
    [...domspans].forEach((e) => {
      let rect = { y: e.offsetTop, x: e.offsetLeft };
      spans[id].x = rect.x;
      spans[id++].y = rect.y;
    });
    tick();
    id = 0;
    [...domspans].forEach((e) => {
      e.style.position = "absolute";
      e.style.top = spans[id].y + "px";
      e.style.left = spans[id++].x + "px";
    });

    function update() {
      spans.forEach((e) => {
        let vec = [curX - e.x, curY - e.y];
        let d = Math.sqrt(Math.pow(vec[0], 2) + Math.pow(vec[1], 2));
        if (d < 100) {
          e.w.set(1000 - d);
        } else {
          e.w.set(100);
        }
      });
    }

    addEventListener("mousemove", (event) => {
      curX = event.clientX;
      curY = event.clientY;
      update();
    });

    /*
    setInterval(() => {
      update();
    }, 10);
    */
  });
</script>

<p>
  {#each spans as span}
    <AniChar w={span.w} char={span.char} />
  {/each}
</p>

<style>
  p {
    color: white;
    cursor: default;
    width: 25vw;
  }
</style>
