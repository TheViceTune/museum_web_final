<template>
  <div class="experience-puzzle-page">
    <!-- Full‑page background -->
    <div class="page-bg"></div>
    <div class="page-overlay"></div>

    <!-- Content -->
    <div class="container page">
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
.container.page {
  position: relative;
  z-index: 1;
}
</style>
