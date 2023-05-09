<template>
    <header id="header" class="marquee-heading">
        <h1 id="first">henri.codes</h1>
        <h1 aria-hidden="true" id="second">henri.codes</h1>
    </header>
</template>
<style>
  .marquee-heading {
      display: grid;
      grid-template-columns: 100vw 100vw;
      width: 100%;

      line-height: 1;
      font-size: 10rem;
      color: var(--text-light);

      > * {
          user-select: none;
      }
  }

  h1#first {
      user-select: none;
      width: fit-content;
      grid-column: 2;
      grid-row: 1;

      animation: move var(--marquee-animation-speed) linear infinite;
  }

  h1#second {
      width: fit-content;
      grid-column: 2;
      grid-row: 1;

      animation: move var(--marquee-animation-speed) linear
      var(--marquee-animation-delay) infinite;
  }
</style>
