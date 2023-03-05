<script lang="ts">
  import { onMount } from "svelte";
  onMount(() => {
    let lines = [...document.getElementsByClassName("line")];

    let p = lines.map((line) => [...line.getElementsByTagName("p")]);

    p.forEach((ps) => {
      ps?.forEach((word) => {
        let text = word.innerText.split("");
        word.innerText = "";
        text.forEach((letter) => {
          const span = document.createElement("span");
          span.innerText = letter;
          span.className = "letter";
          word.appendChild(span);
        });
      });
    });
  });

  function reset(event: Event) {
    let e = event.currentTarget;
    if (e instanceof Element) {
      let spans = [...e.getElementsByTagName("span")];
      spans.forEach((span) => {
        span.style.transform = "";
      });
    }
  }

  let target = "";
  function explode(event: Event) {
    let e = event.currentTarget;
    if (e instanceof Element) {
      if (target != e.id) {
        target = e.id;
        let spans = [...e.getElementsByTagName("span")];
        spans.forEach((span) => {
          let t1 =
            Math.floor(Math.random() * 25) * (Math.random() > 0.5 ? 1 : -1);
          let t2 =
            Math.floor(Math.random() * 25) * (Math.random() > 0.5 ? 1 : -1);
          let r =
            Math.floor(Math.random() * 20) * (Math.random() > 0.5 ? 1 : -1);
          span.style.display = "inline-block";
          span.style.transform = `translate(${t1}%, ${t2}%) rotate(${r}deg)`;
        });
      }
    }
  }
</script>

<svelte:head>
  <title>Home</title>
</svelte:head>

<body>
  <div id="text">
    <a id="name" href="#" on:mouseover={explode} on:mouseleave={reset}>
      <div class="line">
        <p>SAMUEL</p>
        <p>LANDO</p>
      </div>
    </a>
    <div class="line" />
    <a
      id="full-stack-developer"
      href="#"
      on:mouseover={explode}
      on:mouseleave={reset}
    >
      <div class="line">
        <p>FULL</p>
        <p>STACK</p>
      </div>
      <div class="line">
        <p>DEVELOPER</p>
      </div>
    </a>
    <a id="portfolio" href="#" on:mouseover={explode} on:mouseleave={reset}>
      <div class="line">
        <p>PORTFOLIO</p>
      </div>
    </a>
    <div class="line">
      <a id="github" href="#" on:mouseover={explode} on:mouseleave={reset}>
        <p>GITHUB</p>
      </a>
      <a id="resume" href="#" on:mouseover={explode} on:mouseleave={reset}>
        <p>RESUME</p>
      </a>
    </div>
    <a id="contact" href="#" on:mouseover={explode} on:mouseleave={reset}>
      <div class="line">
        <p>CONTACT</p>
      </div>
    </a>
  </div></body
>

<style>
  .line {
    display: flex;
    justify-content: space-between;
    width: 25vw;
  }

  #text {
    width: 25vw;
    display: flex;
    flex-flow: row wrap;
  }

  p {
    color: white;
    font-size: 3vw;
    font-family: "Rubrik", sans-serif;
    margin: 0rem;
  }

  a {
    text-decoration: none;
  }

  #text:has(.line:hover) a:not(:hover) {
    opacity: 0.2;
  }

  body {
    background-color: black;
    margin: 0px;
    width: 100vw;
    height: 100vh;
  }
</style>
