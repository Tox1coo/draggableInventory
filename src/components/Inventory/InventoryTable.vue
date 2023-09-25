<template>
  <div class="inventory">
    <Card v-for="column in columns" :key="column.id"
          @drop="drop($event, column.id)"
          @showInfo="showModalItem"
          :columnId="column.id">
      <Item :class="{'card__item--draggable': draggableItem === getItem(column.id).id}" v-if="getItem(column.id) !== undefined" :item="getItem(column.id)" draggable="true" @dragstart="drag($event, getItem(column.id))" :sizes="{width:4.8, height:4.8}"/>
      <div v-if="getItem(column.id) !== undefined" class="number">
        {{getItem(column.id).id}}
      </div>
    </Card>
    <Modal v-model:show="showModal" :Item="activeItem" @deleteItem="deleteItem"></Modal>

  </div>
</template>

<script lang="ts">
import Card from "./Card.vue";
import Item from "./Item.vue";
import {ref} from "vue"
import Modal from "./Modal.vue";
interface Item {
  id: number,
  columnId: number,
  color: string,
  name: string,
  description:string
}
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
        id: ++i
      }
    })
    let items = ref();
    if(localStorage.getItem('items') !== '') {
      items = ref(JSON.parse(localStorage.getItem('items')));
    }else {
      items = ref<Item[]>([
        {
          id: 4,
          columnId: 1,
          color: '#93BA77',
          name: "name",
          description: "desctiption"
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
    let activeItem = ref<Item>();
    let draggableItem = ref<number>(0);
    const getItem = (id:number) :Item | undefined => {
        const item = items.value.filter((item) => item.columnId === id)[0];
        return item;
    }

    function drag(event: DragEvent, item:Item):void {
      draggableItem.value = item.id;
      console.dir(event.target)
      event.dataTransfer.dropEffect = "move";
      event.dataTransfer.effectAllowed = "move";
      event.dataTransfer.setData("itemId", item.id.toString());

    }
    function drop(event: DragEvent, columnId:number):void {
      draggableItem.value = 0;
      const itemId:number = +event.dataTransfer.getData("itemId");
      items.value = items.value.map((el) => {
        if(itemId === el.id)
          el.columnId = columnId;
        return el;
      })
      localStorage.setItem("items", JSON.stringify(items.value))
    }

    const showModalItem = (columnID:number):void => {
      activeItem.value = getItem(columnID);
      if(activeItem.value === undefined) {
        showModal.value = false
        return
      }
      showModal.value = true;
    }

    const deleteItem = (currentItem:Item):void => {
      const itemIndex:number = items.value.findIndex((item) => {
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