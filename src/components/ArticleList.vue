<template>
  <section class="list">
    <article-card
      v-for="article in articles"
      :key="article.id"
      v-bind="article"
      @liked="$emit('liked', $event)"
      @comment-added="$emit('comment-added', $event)"
    />
  </section>
</template>

<script>
import ArticleCard from "./ArticleCard.vue"

export default {
  name: "ArticleList",
  components: { ArticleCard },
  props: {
    articles: { type: Array, required: true },
  },
  emits: ["liked", "comment-added"],
}
</script>

<style scoped>
.list {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 24px;
  align-items: start;
}

@media (max-width: 1100px) {
  .list { grid-template-columns: repeat(2, minmax(0, 1fr)); }
}

@media (max-width: 720px) {
  .list { grid-template-columns: 1fr; }
}
</style>
