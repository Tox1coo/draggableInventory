<template>
  <div  class="card__item" >
    <div :style="{width:sizes?.width + 'rem', height: sizes?.height + 'rem'}" class="card__item-wrapper">
      <div class="block" :style="{'background': getBackground}"></div>
      <div class="block" :style="{'background': getBackground}"></div>
    </div>
  </div>
</template>

<script lang="ts">
import {computed, PropType} from "vue";

import {Item as ItemInterface, Sizes} from "../../../types/types"


export default {
  name: "Item",
  props: {
    item: {
      type: Object as PropType<ItemInterface>
    },
    sizes: {
      type: Object as PropType<Sizes>
    }
  },
  setup(props) {

    const getBackground = computed<string>(() => props.item?.color || "transparent");

    return {getBackground};
  }
}
</script>

<style lang="scss">
  .card__item {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    &-wrapper {
      position: relative;
      border: 1px solid #4D4D4D;
    }
    &--draggable {
      border: 2px solid #4D4D4D;
      border-radius:20px;
    }
  }
  .card__item .block{
    width: inherit;
    height: inherit;
    position: relative;
    z-index: 0;
    transform: translateZ(-100px);

    &:first-child {
      transform: translateZ(100px);
      position: absolute;
      top: -1rem;
      left: 1rem;
      filter: blur(2px);
      backdrop-filter: blur(15px);
      opacity: 0.6;
    }
  }
</style>