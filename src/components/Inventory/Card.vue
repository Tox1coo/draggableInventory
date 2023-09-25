<template>
  <div @click="$emit('showInfo', columnId)" class="card" @dragenter.prevent @dragover.prevent>
    <slot></slot>
  </div>
</template>

<script lang="ts">
export default {
  name: "Card",
  props: {
    columnId: Number
  }
}
</script>

<style scoped lang="scss">
  .card {
    width: 10.5rem;
    height: 10rem;
    background-color: #262626;
    border: 1px solid #4D4D4D;
    padding: 0;
    position: relative;
  }

</style>