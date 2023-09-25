<template>
  <div class="card" @dragenter.prevent @dragover.prevent>
    <slot></slot>
  </div>
</template>

<script lang="ts">


export default {
  name: "Card",

}
</script>

<style scoped>
  .card {
    width: 10.5rem;
    height: 10rem;
    background-color: #262626;
    border: 1px solid #4D4D4D;
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>