<script lang="ts">
  interface link {
    text: string;
    href: string;
  }

  export let links: link[][] = [];
  export let translate = 25;
  export let rotate = 20;
  export let width = 25;

  // We need to split up the texts.
  let links2 = links.map((line) => {
    return line.map((link) => {
      let chars = link.text
        .split("\n")
        .map((subline) => subline.split(" ").map((word) => word.split("")));
      return { text: chars, href: link.href };
    });
  });

  let target: Element | null;
  function reset(event: Event) {
    target = null;
    let e = event.currentTarget;
    if (e instanceof Element) {
      let spans = [...e.getElementsByTagName("span")];
      spans.forEach((span) => {
        span.style.transform = "";
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
        let spans = [...e.getElementsByTagName("span")];
        spans.forEach((span) => {
          let t1 = _random(translate);
          let t2 = _random(translate);
          let r = _random(rotate);
          span.style.display = "inline-block";
          span.style.transform = `translate(${t1}%, ${t2}%) rotate(${r}deg)`;
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
                    <span>{char}</span>
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

  span {
    color: white;
    font-size: 3vw;
    font-family: "Rubrik", sans-serif;
    margin: 0rem;
  }

  a {
    text-decoration: none;
  }
</style>
