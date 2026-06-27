<template>
  <div class="video-wrapper">
    <video
      ref="videoRef"
      class="home-video"
      controls
      autoplay
      muted
      loop
      playsinline
    >
      <source src="/Hero%20Video.MP4" type="video/mp4" />
      Trình duyệt của bạn không hỗ trợ video.
    </video>
  </div>
</template>

<style scoped>
.video-wrapper {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px 0;
  background: var(--cream);
}
.home-video {
  width: 100%;
  max-width: 900px;
  max-height: 80vh;
  border-radius: var(--radius);
  box-shadow: var(--shadow);
  display: block;
}
</style>
