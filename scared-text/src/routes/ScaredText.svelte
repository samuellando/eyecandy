<script lang="ts">
  import { spring } from "svelte/motion";
  import AniChar from "./AniChar.svelte";
  import type { Spring } from "svelte/motion";
  import { get } from "svelte/store";
  import { onMount, tick } from "svelte";

  export let text: string;

  interface point {
    x: number;
    y: number;
  }

  interface char {
    id: number;
    char: string;
    default: point;
    x: Spring<number>;
    y: Spring<number>;
  }

  let id = 0;
  let spans: char[] = text.split("").map((c) => {
    return {
      id: id++,
      char: c,
      default: { x: 0, y: 0 },
      x: spring(100),
      y: spring(100),
    };
  });

  let curX = 0;
  let curY = 0;

  onMount(() => {
    var domspans = document.getElementsByTagName("span");
    id = 0;
    [...domspans].forEach((e) => {
      let rect = { y: e.offsetTop, x: e.offsetLeft };
      spans[id].default = { x: rect.x, y: rect.y };
      spans[id].x.set(rect.x);
      spans[id++].y.set(rect.y);
    });
    tick();
    id = 0;
    [...domspans].forEach((e) => {
      e.style.position = "absolute";
    });

    function update() {
      spans.forEach((e) => {
        let rect = { x: get(e.x), y: get(e.y) };
        let vec = [curX - rect.x, curY - rect.y];
        let d = Math.sqrt(Math.pow(vec[0], 2) + Math.pow(vec[1], 2));
        if (d < 100) {
          vec[0] = (vec[0] * 110) / d;
          vec[1] = (vec[1] * 110) / d;
          e.x.set(rect.x - vec[0]);
          e.y.set(rect.y - vec[1]);
        } else {
          let def = e.default;
          e.x.set(def.x);
          e.y.set(def.y);
        }
      });
    }

    addEventListener("mousemove", (event) => {
      curX = event.clientX;
      curY = event.clientY;
      update();
    });

    setInterval(() => {
      update();
    }, 10);
  });
</script>

<p>
  {#each spans as span}
    <AniChar x={span.x} y={span.y} char={span.char} />
  {/each}
</p>

<style>
  p {
    color: white;
    cursor: default;
    width: 25vw;
  }
</style>
