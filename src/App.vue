<script lang="ts">
import Sidebar from "./components/UI/Sidebar.vue";
import InventoryTable from "./components/Inventory/InventoryTable.vue";
import Footer from "./components/UI/Footer.vue";
export default {
  name: "App",
  components: {Footer,InventoryTable,Sidebar},
  setup() {
    const countColumnsInventory = 25
    return {countColumnsInventory};
  }
}
</script>

<template>
  <div class="container">
    <main class="main">
      <Sidebar/>
      <InventoryTable :count-column="countColumnsInventory"/>
      <Footer/>
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

</style>
