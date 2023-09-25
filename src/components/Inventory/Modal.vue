<template>
  <Transition name="slide-fade">
    <div v-if="show" class="modal">
        <div class="modal__wrapper info">
          <div @click="$emit('update:show', false)" class="close">
            <div class="close__wrapper">
              <span></span>
              <span></span>
            </div>
          </div>
          <div class="info__image">
            <Item :item="Item" :sizes="{width:13, height:13}"/>
          </div>
          <div class="info__description item-info">
              <h3 class="item-info__title">
                  Lorem Ipsum
              </h3>
              <div class="item-info__description">
                <p>Lorem Ipsum - это текст-"рыба", часто используемый в печати и вэб-дизайне. Lorem Ipsum является стандартной "рыбой" для текстов на латинице с начала XVI века. В то время некий безымянный печатник создал большую коллекцию размеров и форм шрифтов, используя Lorem Ipsum для распечатки образцов. Lorem Ipsum </p>

              </div>
          </div>

          <div class="info__buttons">
            <button @click="$emit('deleteItem', Item)" class="btn">
              Удалить предмет
            </button>
          </div>
        </div>
    </div>
  </Transition>
</template>

<script lang="ts">
import Item from "./Item.vue";
import {PropType} from "vue";
interface Item {
  id: number,
  columnId: number,
  color: string,
  name: string,
  description:string
}
export default {
  name: "Modal",
  components: {Item},
  props: {
    show: {
      type: Boolean,
      default: false
    },
    Item: {
      type: Object as PropType<Item>
    }
  }
}
</script>

<style scoped lang="scss">
.modal {
  height: 100%;
  position: absolute;
  width: 25rem;
  backdrop-filter: blur(5px);
  background-color: rgba(#262626, 0.5);
  right: 0;
  padding: 1.8rem 1.5rem;
  &__wrapper {
    position: relative;
    .close {
      top: -0.5rem;
      right: -0.5rem;
    }
  }
}

.info {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
  &__image {
    flex: 0 1 30%;
    border-bottom: 1px solid #4D4D4D;
    padding: 2rem;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  &__description {
    color: #4D4D4D;
    font-size: 1.2rem;
  }
  &__buttons {
    padding: 1.5rem ;
  }
}

.item-info {
  display: flex;
  flex-direction: column;
  height: 100%;
  border-bottom: 1px solid #4D4D4D;
  &__title {
    margin: 1rem 0rem 2rem;
  }

}

.slide-fade-enter-active {
  transition: all 0.3s ease-out;
}

.slide-fade-leave-active {
  transition: all 0.8s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateX(20px);
  opacity: 0;
}
.btn {
  background-color: #FF8888;
  color: #fff;
  width: 100%;
  padding: 1.1rem 3.5rem;
  font-family: "sans-serif";
  font-weight: 400;
  font-size: 1.4rem;
  border: none;
  transition: none;
}
</style>