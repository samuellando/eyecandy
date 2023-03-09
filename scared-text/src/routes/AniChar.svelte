<script lang="ts">
  import type { Spring } from "svelte/motion";

  export let x: Spring<number>;
  export let y: Spring<number>;
  export let char: string;
</script>

<span style="top: {$y}px; left: {$x}px">{char}</span>
