<script lang="ts">
  import type { Tweened } from "svelte/motion";

  export let t1: Tweened<number>;
  export let t2: Tweened<number>;
  export let r: Tweened<number>;
  export let value: string;

  let t1v: number;
  let t2v: number;
  let rv: number;

  t1.subscribe((value) => {
    t1v = value;
  });

  t2.subscribe((value) => {
    t2v = value;
  });

  r.subscribe((value) => {
    rv = value;
  });

  $: style = `
display: inline-block;
transform: translate(${t1v}%, ${t2v}%) rotate(${rv}deg)
`;
</script>

<span {style}>{value}</span>

<style>
  span {
    color: white;
    font-size: 3vw;
    font-family: "Rubrik", sans-serif;
    margin: 0rem;
  }
</style>
