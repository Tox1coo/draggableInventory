<script lang="ts">
import Sidebar from "./components/UI/Sidebar.vue";
import InventoryTable from "./components/Inventory/InventoryTable.vue";
import Footer from "./components/UI/Footer.vue";
import {ref} from 'vue';
export default {
  name: "App",
  components: {Footer,InventoryTable,Sidebar},
  setup() {
    const countColumnsInventory = 25
    let returnInventory = ref<boolean>(false)
    function updateInventory() {
      returnInventory.value = true;
    }
    return {countColumnsInventory,updateInventory, returnInventory};
  }
}
</script>

<template>
  <div class="container">
    <main class="main">
      <Sidebar/>
      <InventoryTable v-model:return-inventory="returnInventory" :count-column="countColumnsInventory"/>
      <Footer @returnInventory="updateInventory"/>
    </main>
  </div>
</template>

<style lang="scss">
  html {
    font-size: 62.5%;
  }
  *,*::before, *::after {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  body {
    background-color: #1D1D1D;
    min-height: 100vh;
  }
  .container {
    max-width: 81.5rem;
    padding: 0 1.5rem;
    margin: 0 auto;
    display: flex;
  }

  .main {
      display: grid;
      grid-template-columns: 23.6rem 1fr;
      grid-template-rows: 1fr;
      gap: 2.4rem;
    margin: auto;
  }

  .close {
    position: absolute;
    cursor: pointer;
    &__wrapper {
      position: relative;
      width: 2rem;
      height: 2rem;

      span {
        position: absolute;
        top: 50%;
        left: 50%;
        width: 2px;
        height: inherit;
        background-color: #4D4D4D;
        &:first-child {
          transform: translate(-50%,-50%) rotate(45deg);
        }
        &:last-child {
          transform: translate(-50%,-50%) rotate(-45deg);
        }

      }
      transition: transform 0.5s ease;
      &:hover {
        transform: rotate(360deg);
      }
    }
  }

</style>
