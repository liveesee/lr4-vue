<template>
  <section class="section">
    <form class="form" @submit.prevent="publish">
      <div class="field" :class="{ error, focused }">
        <svg width="32" height="32" viewBox="0 0 40 40" aria-hidden="true">
          <circle cx="20" cy="20" r="20" fill="#D8CFBC" />
          <circle cx="20" cy="15" r="7" fill="#565449" />
          <path d="M6.5 34.5c2.8-6.2 8-9.5 13.5-9.5s10.7 3.3 13.5 9.5" fill="#565449" />
        </svg>
        <input
          v-model="draft"
          :placeholder="error ? 'Комментарий не может быть пустым' : 'Напишите вдумчивый ответ...'"
          aria-label="Текст комментария"
          @focus="focused = true"
          @blur="focused = false"
          @keydown.meta.enter="publish"
          @keydown.ctrl.enter="publish"
        />
      </div>
      <p v-if="error" class="error-text">Ошибка · добавьте пару слов</p>
      <button type="submit" class="post">
        <span>Опубликовать</span>
        <svg width="16" height="16" viewBox="0 0 256 256" aria-hidden="true">
          <path fill="currentColor" d="M221.66,133.66l-72,72a8,8,0,0,1-11.32-11.32L196.69,136H40a8,8,0,0,1,0-16H196.69L138.34,61.66a8,8,0,0,1,11.32-11.32l72,72A8,8,0,0,1,221.66,133.66Z" />
        </svg>
      </button>
    </form>
    <p v-if="comments.length === 0" class="empty">Пока нет комментариев</p>
    <ul v-else class="comments">
      <li v-for="comment in comments" :key="comment.id" class="item">
        <p class="author">{{ comment.author }}</p>
        <p class="text">{{ comment.text }}</p>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  name: "CommentSection",
  emits: ["comment-added"],
  data() {
    return {
      draft: "",
      focused: false,
      error: false,
      nextId: 1,
      comments: [],
    }
  },
  methods: {
    publish() {
      const text = this.draft.trim()
      if (!text) {
        this.error = true
        return
      }
      const comment = { id: this.nextId, author: "Вы", text }
      this.nextId += 1
      this.comments.push(comment)
      this.draft = ""
      this.error = false
      this.$emit("comment-added", comment)
    },
  },
  watch: {
    draft(value) {
      if (this.error && value.trim()) this.error = false
    },
  },
}
</script>

<style scoped>
.section { width: 100%; margin-top: 4px; }

.field {
  height: 44px;
  box-sizing: border-box;
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 0 14px 0 8px;
  border: 1px solid var(--border);
  border-radius: 16px;
  background: var(--paper);
}

.field svg {
  flex: 0 0 auto;
  width: 22px;
  height: 22px;
}

.field input {
  flex: 1;
  min-width: 0;
  border: 0;
  outline: none;
  background: transparent;
  color: var(--ink);
  font-size: 14px;
}

.field input::placeholder { color: var(--muted); }
.focused { border: 1.5px solid var(--olive); }
.error { border: 1.5px solid var(--error); }
.error input, .error input::placeholder { color: var(--error); }

.error-text, .empty {
  margin: 8px 0 0;
  font-size: 12px;
}

.empty { color: var(--muted); }
.error-text { color: var(--error); }

.post {
  height: 44px;
  margin-top: 12px;
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 0 18px;
  border: 0;
  border-radius: 14px;
  background: var(--ink);
  color: var(--paper);
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
}

.post:hover { background: var(--olive-soft); }

.comments {
  list-style: none;
  margin: 16px 0 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.item {
  padding: 12px 14px;
  border-radius: 14px;
  background: rgba(216, 207, 188, 0.45);
}

.author { margin: 0; font-size: 13px; font-weight: 500; }
.text { margin: 4px 0 0; color: var(--olive); font-size: 14px; line-height: 20px; }
</style>
