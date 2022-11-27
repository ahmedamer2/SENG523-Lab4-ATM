<script setup>
import ATM from "./components/ATM.vue";
</script>

<template>
  <body>
    <header id="header">
      <div class="title">SENG 523 - ATM Application</div>
      <div class="subtitle">
        Ahmed Amer, Ian Kang, Luke Hollinda, Brian Kramer, Deivydas Johnson
      </div>
    </header>
    <main id="main">
      <ATM></ATM>
    </main>
  </body>
</template>

<style lang="less" scoped>
#header {
  padding: 1.2em;
  text-align: center;
  font-family: "Barlow-Regular", Arial, Helvetica, sans-serif;

  .title {
    font-family: "Barlow-Bold";
    font-size: 3rem;
    margin-bottom: 1rem;
  }

  .subtitle {
    font-family: "Barlow-Medium";
    font-size: 1.5rem;
  }
}

#main {
  height: calc(100% - 8.9rem);
}
</style>
