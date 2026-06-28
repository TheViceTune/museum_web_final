<template>
  <div class="artifact-display">
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
</template>

<script setup>
defineProps(["exhibit"]);

function handleImageError(e) {
  e.target.style.display = "none";
}
</script>

<style scoped>
.artifact-display {
  background: var(--primary-dark);
  border-radius: var(--radius);
  padding: 24px;
  box-shadow: var(--shadow);
  min-height: 400px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: var(--white);
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
  color: var(--white);
  opacity: 0.8;
  margin-top: 4px;
}
</style>
