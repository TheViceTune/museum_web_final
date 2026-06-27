<template>
  <div class="artifact-display">
    <div class="artifact-image-box">
      <img
        v-if="exhibit.image"
        :src="exhibit.image"
        :alt="exhibit.label"
        class="artifact-image"
        loading="lazy"
        @error="handleImageError"
      />
      <div v-else class="icon-placeholder">{{ exhibit.icon }}</div>
      <p class="image-note">{{ exhibit.imageNote }}</p>
    </div>
  </div>
</template>

<script setup>
defineProps(["exhibit"]);

function handleImageError(e) {
  e.target.style.display = "none";
}
</script>

<style scoped>
.artifact-display {
  background: var(--white);
  border-radius: var(--radius);
  padding: 24px;
  box-shadow: var(--shadow);
  min-height: 400px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.artifact-image-box {
  background: var(--cream);
  border-radius: var(--radius);
  padding: 20px;
  text-align: center;
  /* Removed border: 2px dashed var(--primary-light); */
  min-height: 240px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: var(--text-light);
  font-size: 14px;
}
.artifact-image {
  width: 100%;
  max-height: 280px;
  object-fit: contain;
  border-radius: var(--radius);
  margin-bottom: 8px;
}
.icon-placeholder {
  font-size: 80px;
  margin-bottom: 8px;
  opacity: 0.6;
}
.image-note {
  font-size: 12px;
  color: var(--text-light);
  margin-top: 4px;
}
</style>
