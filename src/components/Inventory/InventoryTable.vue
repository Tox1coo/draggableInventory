<template>
  <div class="inventory">
    <Card v-for="column in columns" :key="column.id"
          @drop="drop($event, column.id)"
          @showInfo="showModalItem"
          :columnId="column.id">
      <Item :class="{'card__item--draggable': draggableItem === getItem(column.id)?.id}" v-if="getItem(column.id) !== undefined" :item="getItem(column.id)" draggable="true" @dragstart="drag($event, getItem(column.id))" :sizes="{width:4.8, height:4.8}"/>
      <div v-if="getItem(column.id) !== undefined" class="number">
        {{getItem(column.id)?.id}}
      </div>
    </Card>
    <Modal v-model:show="showModal" :Item="activeItem" @deleteItem="deleteItem"></Modal>

  </div>
</template>

<script lang="ts">
import {ref} from "vue"
import {Item as ItemInterface} from "../../../types/types"

import Card from "./Card.vue";
import Item from "./Item.vue";
import Modal from "./Modal.vue";

export default {
  name: "InventoryTable",
  components: {Modal, Card,Item},
  props: {
    countColumn: {
      type: Number,
      default: 25
    }
  },
  setup(props) {
    const columns = Array.apply(null, Array(props.countColumn)).map((y,i) =>  {
      return {
        id: ++i,
        y: y
      }
    })
    let items = ref();
    if(localStorage.getItem('items')) {
      items = ref(JSON.parse(localStorage.getItem('items') || '{}'));
    }else {
      items = ref<ItemInterface[]>([
        {
          id: 4,
          columnId: 1,
          color: '#93BA77',
          name: "name",
          description: "desctiptiopn"
        },
        {
          id: 2,
          columnId: 2,
          color: '#BAA376',
          name: "name",
          description: "desctiption"
        },
        {
          id: 5,
          columnId: 3,
          color: '#6A73C1',
          name: "name",
          description: "desctiption"
        }
      ]);
    }
    let showModal = ref<boolean>(false);
    let activeItem = ref<ItemInterface>();
    let draggableItem = ref<number>(0);
    const getItem = (id:number) :ItemInterface  => {
        const item = items.value.filter((item:ItemInterface) => item.columnId === id)[0];
        return item;
    }

    function drag(event: DragEvent, item:ItemInterface):void {
      draggableItem.value = item.id;
      if(event.dataTransfer) {
        event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("itemId", item.id.toString());
      }


    }
    function drop(event: DragEvent, columnId:number):void {
      draggableItem.value = 0;
      if(event.dataTransfer) {
        const itemId:number = +event.dataTransfer.getData("itemId");
        items.value = items.value.map((el:ItemInterface) => {
          if(itemId === el.id)
            el.columnId = columnId;
          return el;
        })
        localStorage.setItem("items", JSON.stringify(items.value))
      }
     
    }

    const showModalItem = (columnID:number):void => {
      activeItem.value = getItem(columnID);
      if(activeItem.value === undefined) {
        showModal.value = false
        return
      }
      showModal.value = true;
    }

    const deleteItem = (currentItem:ItemInterface):void => {
      const itemIndex:number = items.value.findIndex((item:ItemInterface) => {
        return currentItem.id === item.id;
      })

      items.value.splice(itemIndex, 1);
      showModal.value = false;
      delete activeItem.value;
      localStorage.setItem("items", JSON.stringify(items.value))
    }
    return {columns, items, getItem,drag,drop,showModal,activeItem,showModalItem,deleteItem,draggableItem}
  }
}
</script>

<style scoped lang="scss">
.inventory {
  display: flex;
  flex-wrap: wrap;
  position: relative;
}

.number {
  position: absolute;
  bottom: 0;
  right: 0;
  padding: 0.2rem 0.4rem;
  font-weight: 500;
  color: #4D4D4D;
  border: 1px solid #4D4D4D;
  border-top-left-radius: 6px;
}
</style>