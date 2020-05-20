<template>
  <header>
    <h1>Restaurant Manager</h1>
  </header>
</template>

<script>
export default {
  name: "Header"
}
</script>

<style scoped>
  header {
    background: #35495e;
    color: #fff;
    text-align: center;
    padding: 1px;
    position: fixed;
    width: 100%;
    top: 0;
    z-index: 999;
  }
  h1 {
    font-size: 1.5rem;
    color: #fdfffe;
    font-weight: 300;
}
</style>