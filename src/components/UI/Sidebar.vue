<template>
  <div class="sidebar">
      <div class="sidebar__image">
        <img :src="imageUrl" alt="sidebar image">
        <h3 class="sidebar__title">
          Lorem ipsum
        </h3>
        <p class="sidebar__description">
          Lorem Ipsum - это текст-"рыба", часто используемый в печати и вэб-дизайне. Lorem Ipsum является стандартной "рыбой" для текстов на латинице с начала XVI века. В то время некий безымянный печатник создал
        </p>
      </div>
  </div>
</template>

<script>
import imageUrl from '/src/assets/sidebar.png' // => or relative path
export default {
  name: "Sidebar",
  setup() {
    return {imageUrl}
  }
}
</script>

<style scoped lang="scss">
.sidebar {
  background-color: #262626;
  border: 1px solid #4D4D4D;
  padding: 1.8rem 1.4rem;

  color: #4D4D4D;
  font-size: 1.3rem;
  &__title,&__description {
    margin-top: 1rem;
  }
}
</style>