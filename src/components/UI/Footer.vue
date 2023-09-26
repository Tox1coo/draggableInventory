<template>
  <div class="footer">
    <p class="sidebar__description">
      Lorem Ipsum - это текст-"рыба", часто используемый в печати и вэб-дизайне. Lorem Ipsum является стандартной "рыбой" для текстов на латинице с начала XVI века. В то время некий безымянный печатник создал
    </p>
    <div class="close">
      <div class="close__wrapper">
        <span></span>
        <span></span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: "Footer"
}
</script>

<style scoped lang="scss">
  .footer {
    background-color: #262626;
    border: 1px solid #4D4D4D;
    padding: 1.8rem 6.6rem 1.8rem 1.8rem;
    grid-column: -3/-1;
    color: #4D4D4D;
    font-size: 1.2rem;
    position: relative;
    .close {
      top: 1rem;
      right: 1rem;
    }
  }
</style>