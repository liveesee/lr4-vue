<template>
  <article class="card">
    <img class="cover" :src="imageUrl" alt="" />
    <div class="body">
      <div class="meta">
        <svg width="32" height="32" viewBox="0 0 40 40" aria-hidden="true">
          <circle cx="20" cy="20" r="20" fill="#D8CFBC" />
          <circle cx="20" cy="15" r="7" fill="#565449" />
          <path d="M6.5 34.5c2.8-6.2 8-9.5 13.5-9.5s10.7 3.3 13.5 9.5" fill="#565449" />
        </svg>
        <div>
          <p class="author">{{ author }}</p>
          <p class="minutes">{{ minutes }} мин</p>
        </div>
      </div>
      <h2 class="title">{{ title }}</h2>
      <p class="excerpt">{{ previewText }}</p>
      <div class="actions">
        <button type="button" class="like" aria-label="Нравится" @click="like">
          <svg width="18" height="18" viewBox="0 0 256 256" aria-hidden="true">
            <path fill="currentColor" d="M240,94c0,70-103.79,126.66-108.21,129a8,8,0,0,1-7.58,0C115.79,220.66,12,164,12,94A62.07,62.07,0,0,1,74,32c20.65,0,38.73,8.88,50,23.89C135.27,40.88,153.35,32,174,32A62.07,62.07,0,0,1,240,94Z" />
          </svg>
          <span class="count">{{ likes }}</span>
        </button>
        <div class="trailing">
        <button type="button" class="comment" :aria-expanded="commentsOpen" @click="commentsOpen = !commentsOpen">
          <svg width="18" height="18" viewBox="0 0 256 256" aria-hidden="true">
            <path fill="currentColor" d="M216,48H40A16,16,0,0,0,24,64V224a15.85,15.85,0,0,0,9.24,14.5A16.13,16.13,0,0,0,40,240a15.89,15.89,0,0,0,10.25-3.78l.09-.07L83,208H216a16,16,0,0,0,16-16V64A16,16,0,0,0,216,48Zm0,144H80a8,8,0,0,0-5.23,1.95L40,224V64H216Z" />
          </svg>
          <span>Комментарий</span>
        </button>
        <button type="button" class="share" aria-label="Поделиться">
          <svg width="16" height="16" viewBox="0 0 256 256" aria-hidden="true">
            <path fill="currentColor" d="M176,160a39.89,39.89,0,0,0-28.62,12.09l-46.1-29.63a39.8,39.8,0,0,0,0-28.92l46.1-29.63a40,40,0,1,0-8.66-13.45l-46.1,29.63a40,40,0,1,0,0,55.88l46.1,29.63A40,40,0,1,0,176,160Zm0-128a24,24,0,1,1-24,24A24,24,0,0,1,176,32ZM64,152a24,24,0,1,1,24-24A24,24,0,0,1,64,152Zm112,72a24,24,0,1,1,24-24A24,24,0,0,1,176,224Z" />
          </svg>
        </button>
        </div>
      </div>
      <comment-section v-show="commentsOpen" @comment-added="$emit('comment-added', $event)" />
    </div>
  </article>
</template>

<script>
import CommentSection from "./CommentSection.vue"

export default {
  name: "ArticleCard",
  components: { CommentSection },
  props: {
    id: { type: Number, required: true },
    title: { type: String, required: true },
    author: { type: String, required: true },
    previewText: { type: String, required: true },
    imageUrl: { type: String, required: true },
    minutes: { type: Number, required: true },
  },
  emits: ["liked", "comment-added"],
  data() {
    return { likes: 0, commentsOpen: false }
  },
  methods: {
    like() {
      this.likes += 1
      this.$emit("liked", this.id)
    },
  },
}
</script>

<style scoped>
.card {
  width: 100%;
  background: var(--paper);
  border: none;
  border-radius: 22px;
  overflow: hidden;
  box-shadow: 0 10px 28px -4px rgba(17, 18, 13, 0.12);
}

.cover {
  display: block;
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.body {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 12px;
  padding: 20px;
}

.meta { display: flex; align-items: center; gap: 10px; }
.author, .minutes { margin: 0; }
.author { font-size: 13px; font-weight: 500; }
.minutes { color: var(--muted); font-size: 12px; }

.title {
  margin: 0;
  font-family: var(--serif);
  font-size: 28px;
  font-weight: 500;
  line-height: 34px;
}

.excerpt {
  margin: 0;
  color: var(--olive);
  font-size: 14px;
  line-height: 22px;
}

.actions {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  gap: 8px;
}

.trailing {
  display: flex;
  align-items: center;
  gap: 8px;
}

.like, .comment, .share {
  height: 44px;
  box-sizing: border-box;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 0 16px;
  border-radius: 14px;
  font-size: 14px;
  font-weight: 500;
  text-decoration: none;
  cursor: pointer;
}

.like {
  gap: 6px;
  padding: 0 10px;
  border: 1px solid var(--like);
  background: var(--paper);
  color: var(--like);
}

.like:hover { background: var(--like); color: var(--paper); }

.comment, .share {
  border: 0;
  background: var(--bone);
  color: var(--ink);
}

.comment:hover, .share:hover { background: var(--olive); color: var(--paper); }

.count {
  display: inline-block;
  width: 1.4em;
  text-align: center;
  font-variant-numeric: tabular-nums;
  flex: 0 0 auto;
}
</style>
