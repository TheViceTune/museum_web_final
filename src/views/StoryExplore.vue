<template>
  <div class="story-page">
    <!-- Fixed background -->
    <div class="page-bg"></div>
    <div class="page-overlay"></div>

    <!-- Content -->
    <div class="page-content container">
      <h1 class="page-title" v-slide-in:left>Ký ức lịch sử</h1>
      <p class="page-subtitle" v-slide-in:left="{ delay: 100 }">
        Lắng nghe những câu chuyện về con người, sự kiện và dấu mốc lịch sử phía
        sau mỗi hiện vật và chiến công trên bầu trời Tổ quốc.
      </p>

      <div class="story-grid" v-slide-in:bottom>
        <router-link
          v-for="story in stories"
          :key="story.id"
          :to="`/kham-pha/ky-uc/${story.id}`"
          class="story-card"
        >
          <div
            class="story-card-bg"
            :style="{
              backgroundImage: `url(${encodeImagePath(story.image) || 'https://via.placeholder.com/400x400/195484/ffffff?text=' + story.title})`,
            }"
          ></div>
          <div class="story-card-overlay"></div>
          <div class="story-card-content">
            <h3>{{ story.title }}</h3>
            <p>{{ story.excerpt }}</p>
            <span class="btn-gold">KHÁM PHÁ →</span>
          </div>
        </router-link>
      </div>

      <div style="text-align: center; margin-top: 20px">
        <router-link to="/kham-pha" class="btn-outline"
          >← Quay lại trang chính</router-link
        >
      </div>
    </div>
  </div>
</template>

<script setup>
import { storyCards } from "@/data/storyData";
const stories = storyCards;

// Helper to URL‑encode image paths (handles spaces and Vietnamese characters)
function encodeImagePath(path) {
  if (!path) return "";
  // Split path into parts and encode each part separately
  const parts = path.split("/");
  return parts
    .map((part, index) => (index === 0 ? part : encodeURIComponent(part)))
    .join("/");
}
</script>

<style scoped>
/* ... (all styles unchanged, but ensure the gradient is reversed) ... */
.story-page {
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

.page-content {
  position: relative;
  z-index: 1;
  padding: 40px 0 20px;
}

.page-title {
  font-size: 32px;
  font-weight: 700;
  color: var(--white);
  text-shadow: 0 2px 12px rgba(0, 0, 0, 0.5);
  margin-bottom: 8px;
  font-family: var(--font-title);
}
.page-subtitle {
  color: var(--white);
  opacity: 0.9;
  font-size: 16px;
  margin-bottom: 30px;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
}

.story-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 30px;
  margin-top: 20px;
}
.story-card {
  position: relative;
  border-radius: var(--radius);
  overflow: hidden;
  height: 350px;
  text-decoration: none;
  color: var(--white);
  display: block;
  box-shadow: var(--shadow);
  transition: var(--transition);
}
.story-card:hover {
  transform: translateY(-6px);
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.25);
}
.story-card-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
}
/* ✅ REVERSED GRADIENT: dark at bottom (for text), clear at top */
.story-card-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    transparent 0%,
    rgba(0, 0, 0, 0.2) 50%,
    rgba(0, 0, 0, 0.8) 100%
  );
}
.story-card-content {
  position: relative;
  z-index: 2;
  padding: 24px;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}
.story-card-content h3 {
  font-family: var(--font-title);
  font-size: 20px;
  letter-spacing: 0.5px;
  margin-bottom: 8px;
}
.story-card-content p {
  font-size: 14px;
  opacity: 0.9;
  margin-bottom: 12px;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.story-card-content .btn-gold {
  background: var(--gold);
  color: var(--primary-dark);
  padding: 6px 20px;
  border-radius: 30px;
  font-weight: 600;
  font-size: 13px;
  align-self: flex-start;
  transition: var(--transition);
}
.story-card-content .btn-gold:hover {
  background: var(--gold-light);
}
@media (max-width: 600px) {
  .story-card {
    height: 280px;
  }
}
</style>
