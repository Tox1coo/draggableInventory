<template>
  <div  class="card__item" >
    <div :style="{width:sizes.width + 'rem', height: sizes.height + 'rem'}" class="card__item-wrapper">
      <div class="block" :style="{'background': getBackgroundSecond}"></div>
      <div class="block" :style="{'background': getBackground}"></div>
    </div>
  </div>
</template>

<script lang="ts">
import {computed, PropType} from "vue";

interface Item {
  id: number,
  columnId: number,
  color: string,
  name: string,
  description:string
}
interface Sizes {
  width: number,
  height: number,
}
export default {
  name: "Item",
  props: {
    item: {
      type: Object as PropType<Item>
    },
    sizes: {
      type: Object as PropType<Sizes>
    }
  },
  setup(props) {
    function shadeColor(color:string, percent:number) {
      let R:number = parseInt(color.substring(1,3),16);
      let G:number = parseInt(color.substring(3,5),16);
      let B:number = parseInt(color.substring(5,7),16);
      R = R * (100 + percent) / 100;
      G = G * (100 + percent) / 100;
      B = B * (100 + percent) / 100;
      R = (R<255)?R:255;
      G = (G<255)?G:255;
      B = (B<255)?B:255;
      R = Math.round(R)
      G = Math.round(G)
      B = Math.round(B)
      let RR = ((R.toString(16).length==1)?"0"+R.toString(16):R.toString(16));
      let GG = ((G.toString(16).length==1)?"0"+G.toString(16):G.toString(16));
      let BB = ((B.toString(16).length==1)?"0"+B.toString(16):B.toString(16));
      return "#"+RR+GG+BB;
    }
    console.log(props)
    const getBackground = computed<string>(() => props.item?.color || "transparent");
    const getBackgroundSecond = computed<string>(() => {
      if(props.item?.color !== undefined) {
        return shadeColor(props.item.color, 10);
      }
      return "transparent";
    })
    return {getBackground,getBackgroundSecond};
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