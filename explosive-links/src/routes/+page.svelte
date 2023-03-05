<script lang="ts">
  import ExplodingMenu from "./ExplodingMenu.svelte";
</script>

<svelte:head>
  <title>Home</title>
</svelte:head>

<body>
  <ExplodingMenu
    links={[
      [{ text: "SAMUEL LANDO", href: "#" }],
      [{ text: "FULL STACK\nDEVELOPER", href: "#" }],
      [{ text: "PORTFOLIO", href: "#" }],
      [
        { text: "GITHUB", href: "#" },
        { text: "RESUME", href: "#" },
      ],
      [{ text: "CONTACT", href: "#" }],
    ]}
  />
</body>

<style>
  body {
    background-color: black;
    margin: 0px;
    width: 100vw;
    height: 100vh;
  }
</style>
