<template>
  <div class="story-detail-page">
    <!-- Full‑page background -->
    <div class="page-bg"></div>
    <div class="page-overlay"></div>

    <!-- Content -->
    <div class="container page">
      <div class="story-detail" v-if="story">
        <h1 class="page-title" v-slide-in:left>{{ story.title }}</h1>
        <div
          class="story-content"
          v-html="story.fullStory"
          v-slide-in:bottom
        ></div>

        <div class="story-navigation" v-slide-in:bottom>
          <router-link
            :to="`/kham-pha/ky-uc/${prevId}`"
            v-if="prevId"
            class="btn-outline story-nav-btn"
          >
            ← Trước
          </router-link>
          <router-link to="/kham-pha/ky-uc" class="btn-outline story-nav-btn">
            📖 Danh sách
          </router-link>
          <router-link
            :to="`/kham-pha/ky-uc/${nextId}`"
            v-if="nextId"
            class="btn-outline story-nav-btn"
          >
            Sau →
          </router-link>
        </div>
      </div>

      <div v-else class="not-found">
        <p>Không tìm thấy câu chuyện.</p>
        <router-link to="/kham-pha/ky-uc" class="btn-outline story-nav-btn"
          >Quay lại</router-link
        >
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, watch, nextTick } from "vue";
import { useRoute } from "vue-router";
import { storyCards } from "@/data/storyData";

const route = useRoute();

const currentIndex = computed(() => {
  return storyCards.findIndex((s) => s.id === route.params.id);
});

const story = computed(() => {
  if (currentIndex.value === -1) return null;
  return storyCards[currentIndex.value];
});

const prevId = computed(() => {
  const idx = currentIndex.value;
  if (idx > 0) return storyCards[idx - 1].id;
  return null;
});

const nextId = computed(() => {
  const idx = currentIndex.value;
  if (idx < storyCards.length - 1) return storyCards[idx + 1].id;
  return null;
});

watch(
  () => route.params.id,
  () => {
    nextTick(() => {
      window.scrollTo({ top: 0, behavior: "smooth" });
    });
  },
);
</script>

<style scoped>
/* Full‑page background */
.story-detail-page {
  position: relative;
  min-height: 100vh;
  width: 100%;
}

.page-bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
  background-image: url("/museum_photos/nen kham pha.png");
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  filter: blur(8px) brightness(0.7);
  transform: scale(1.05);
}

.page-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  background: rgba(0, 0, 0, 0.3);
}

.container.page {
  position: relative;
  z-index: 1;
  padding: 40px 20px;
}

.page-title {
  font-family: var(--font-title);
  font-size: 36px;
  color: var(--white);
  text-shadow: 0 2px 12px rgba(0, 0, 0, 0.5);
  margin-bottom: 20px;
  letter-spacing: 1px;
}

.story-detail {
  max-width: 800px;
  margin: 0 auto;
}

.story-content {
  font-size: 16px;
  line-height: 1.9;
  color: var(--text);
  background: var(--white);
  padding: 30px;
  border-radius: var(--radius);
  box-shadow: var(--shadow);
}

.story-content p {
  margin-bottom: 16px;
  text-align: left;
}
.story-content h4 {
  font-weight: 700;
  margin-top: 20px;
  text-align: left;
}

.story-content :deep(.story-image-wrapper) {
  margin: 24px auto;
  text-align: center;
  max-width: 100%;
}

.story-content :deep(.story-image-wrapper img) {
  max-width: 100%;
  height: auto;
  border-radius: 8px;
  display: inline-block;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.story-content :deep(.story-image-wrapper figcaption),
.story-content :deep(.story-image-wrapper .caption) {
  font-style: italic !important;
  font-size: 14px;
  color: var(--text-light);
  text-align: center;
  margin-top: 6px;
  display: block;
}

/* Navigation buttons – custom gold style */
.story-nav-btn {
  border-color: #fada2d !important;
  color: #fada2d !important;
  background: transparent !important;
}

.story-nav-btn:hover {
  background: #fada2d !important;
  color: var(--primary-dark) !important;
  border-color: #fada2d !important;
}

.story-navigation {
  display: flex;
  justify-content: space-between;
  gap: 16px;
  margin-top: 30px;
  flex-wrap: wrap;
}
.story-navigation .btn-outline {
  padding: 8px 20px;
}

.not-found {
  text-align: center;
  padding: 40px 0;
}

@media (max-width: 600px) {
  .story-navigation {
    flex-direction: column;
    align-items: stretch;
  }
  .story-navigation .btn-outline {
    text-align: center;
  }
  .page-title {
    font-size: 28px;
  }
}
</style>
