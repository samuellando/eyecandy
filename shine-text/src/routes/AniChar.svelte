<script lang="ts">
  import type { Spring } from "svelte/motion";

  export let w: Spring<number>;
  export let char: string;
</script>

<span style="font-weight: {$w}">{char}</span>
