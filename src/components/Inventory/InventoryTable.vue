<template>
  <div class="inventory">
    <Card v-for="column in columns" :key="column.id" @drop="drop($event, column.id)">
      <Item :item="getItem(column.id)" draggable="true" @dragstart="drag($event, getItem(column.id))"/>

    </Card>
  </div>
</template>

<script lang="ts">
import Card from "./Card.vue";
import Item from "./Item.vue";
import {ref} from "vue"
interface Item {
  id: number,
  columnId: number,
  color: string
}
export default {
  name: "InventoryTable",
  components: {Card,Item},
  props: {
    countColumn: {
      type: Number,
      default: 25
    }
  },
  setup(props) {
    const columns = Array.apply(null, Array(props.countColumn)).map((y,i) =>  {
      return {
        id: ++i
      }
    })

    const items = ref<Item[]>([
      {
        id:4,
        columnId:1,
        color: '#93BA77'
      },
      {
        id:2,
        columnId:2,
        color: '#BAA376'
      },
      {
        id:5,
        columnId:3,
        color: '#6A73C1'
      }
    ]);

    const getItem = (id:number) :Item | undefined => {
        const item = items.value.filter((item) => item.columnId === id)[0];
        return item;
    }

    function drag(event: DragEvent, item:Item):void {
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("itemId", item.id.toString());

    }
    function drop(event: DragEvent, columnId:number):void {
      const itemId:number = +event.dataTransfer.getData("itemId");
      items.value = items.value.map((el) => {
        if(itemId === el.id)
          el.columnId = columnId;
        return el;

      })

    }
    return {columns, items, getItem,drag,drop}
  }
}
</script>

<style scoped>
.inventory {
  display: flex;
  flex-wrap: wrap;
}
</style>