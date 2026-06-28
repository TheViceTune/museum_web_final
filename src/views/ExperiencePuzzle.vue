<template>
  <div class="experience-puzzle-page">
    <!-- Full‑page background -->
    <div class="page-bg"></div>
    <div class="page-overlay"></div>

    <!-- Content -->
    <div class="page-content container">
      <h1 class="page-title" v-slide-in:left>Truy tìm mảnh ghép</h1>
      <p class="page-subtitle" v-slide-in:left="{ delay: 100 }">
        Hãy thử thách bản thân với những câu hỏi thú vị về quân chủng Phòng
        không - Không quân để học hỏi thêm nhiều kiến thức mới nhé!
      </p>

      <PuzzleGame v-slide-in:bottom="{ delay: 150 }" />

      <div style="text-align: center; margin-top: 30px">
        <router-link to="/trai-nghiem" class="btn-outline"
          >← Quay lại trang chính</router-link
        >
      </div>
    </div>
  </div>
</template>

<script setup>
import PuzzleGame from "@/components/PuzzleGame.vue";
</script>

<style scoped>
.experience-puzzle-page {
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
  background-image: url("/museum_photos/experience/nền.png");
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
  padding: 40px 20px;
  max-width: 1200px;
  margin: 0 auto;
}

/* Title & subtitle styles – matching ExperienceHome and ExploreHome */
.page-title {
  font-family: var(--font-title);
  font-size: 48px;
  color: var(--white);
  text-shadow: 0 2px 12px rgba(0, 0, 0, 0.5);
  margin-bottom: 12px;
  letter-spacing: 2px;
  text-align: center;
}
.page-subtitle {
  font-family: var(--font-body);
  font-size: 18px;
  color: var(--white);
  opacity: 0.9;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
  margin-bottom: 40px;
  max-width: 700px;
  margin-left: auto;
  margin-right: auto;
  text-align: center;
}

@media (max-width: 768px) {
  .page-title {
    font-size: 28px;
  }
  .page-subtitle {
    font-size: 16px;
    padding: 0 20px;
  }
}
</style>
