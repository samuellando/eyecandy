<script lang="ts">
  import { tweened } from "svelte/motion";
  import type { Tweened } from "svelte/motion";
  import { cubicOut } from "svelte/easing";
  import AniChar from "./AniChar.svelte";

  interface link {
    text: string;
    href: string;
  }

  interface chardata {
    t1: Tweened<number>;
    t2: Tweened<number>;
    r: Tweened<number>;
    char: string;
    id: number;
  }

  export let links: link[][] = [];
  export let translate = 50;
  export let rotate = 75;
  export let width = 25;
  export let duration = 1000;

  function _tweened() {
    return tweened(0, {
      duration: duration,
      easing: cubicOut,
    });
  }

  console.log(_tweened());

  // We need to split up the texts.
  let id = 0;
  let charData: chardata[] = [];
  let links2 = links.map((line) => {
    return line.map((link) => {
      let chars = link.text.split("\n").map((subline) =>
        subline.split(" ").map((word) =>
          word.split("").map((c) => {
            let data = {
              id: id++,
              char: c,
              t1: _tweened(),
              t2: _tweened(),
              r: _tweened(),
            };
            charData[data.id] = data;
            return data;
          })
        )
      );
      return { text: chars, href: link.href };
    });
  });

  let target: Element | null;
  function reset(event: Event) {
    target = null;
    let e = event.currentTarget;
    if (e instanceof Element) {
      let chars = [...e.getElementsByClassName("char")];
      chars.forEach((char) => {
        let data = charData[parseInt(char.id)];
        data.t1.set(0);
        data.t2.set(0);
        data.r.set(0);
      });
    }
  }

  function _random(n: any) {
    return Math.floor(Math.random() * n) * (Math.random() > 0.5 ? 1 : -1);
  }

  function explode(event: Event) {
    let e = event.currentTarget;
    if (e instanceof Element) {
      if (target != e) {
        target = e;
        let chars = [...e.getElementsByClassName("char")];
        chars.forEach((char) => {
          let t1 = _random(translate);
          let t2 = _random(translate);
          let r = _random(rotate);
          let data = charData[parseInt(char.id)];
          data.t1.set(t1);
          data.t2.set(t2);
          data.r.set(r);
        });
      }
    }
  }
</script>

<div id="text" style="width: {width}vw">
  {#each links2 as line}
    <div class="line">
      {#each line as anchor}
        <a href={anchor.href} on:mouseenter={explode} on:mouseleave={reset}>
          {#each anchor.text as line}
            <div class="sub-line">
              {#each line as word}
                <div class="word">
                  {#each word as char}
                    <div class="char" id={char.id.toString()}>
                      <AniChar
                        t1={char.t1}
                        t2={char.t2}
                        r={char.r}
                        value={char.char}
                      />
                    </div>
                  {/each}
                </div>
              {/each}
            </div>
          {/each}
        </a>
      {/each}
    </div>
  {/each}
</div>

<style>
  /* Make the lines stack up */
  #text {
    display: flex;
    flex-flow: row wrap;
  }
  .word {
    display: flex;
    flex-flow: row;
  }
  /* On each line, spread out the elements to take up the entire space. */
  .line {
    display: flex;
    justify-content: space-between;
    flex-grow: 1;
  }
  .line a {
    flex-grow: 1;
  }
  .sub-line {
    display: flex;
    justify-content: space-between;
  }
  /* When a line is being hovered, make all the non hovered ones fade */
  #text:has(.line:hover) a:not(:hover) {
    opacity: 0.2;
  }

  a {
    text-decoration: none;
  }
</style>
