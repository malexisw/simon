<template>
  <div id="app">
      <Simon> </Simon>
  </div>
</template>

<script>
import Simon from './components/Simon'

export default {
  name: 'App',
  components: {
    Simon
  }
}
</script>

<style>
*{
    margin: 0;
    padding: 0;
    border: 0;
    font: inherit;
    vertical-align: baseline;
    overflow: hidden;
}

:root {
    font-size: 16px;
    font-weight: normal;
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.6rem;
    --color-first: #dfe7ef;
    --color-second:#eef3f7;
    --color-third:#24292e;
    --text-first:#acb8cc;
    --text-second:#71829e;
}

#app {
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  display: flex;
  align-items: center;
  justify-content: center;

  width: 100%;
  height: 100vh;
  position: relative;

  background: var(--color-first);
}


</style>
