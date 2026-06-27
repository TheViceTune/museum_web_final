<template>
  <div class="explore-page">
    <!-- Background with blur overlay – fixed to cover entire viewport -->
    <div class="explore-bg"></div>
    <div class="explore-overlay"></div>

    <!-- Content – scrolls normally -->
    <div class="explore-content">
      <h1 class="page-title" v-slide-in:left>VÙNG TRỜI KÝ ỨC</h1>
      <p class="page-subtitle" v-slide-in:left="{ delay: 100 }">
        Tái hiện những câu chuyện, hiện vật và ký ức của lực lượng Phòng không -
        Không quân Việt Nam trên hành trình bảo vệ Tổ Quốc
      </p>

      <div class="explore-cards" v-slide-in:bottom>
        <router-link to="/kham-pha/hien-vat" class="exp-card">
          <div
            class="exp-card-bg"
            style="
              background-image: url(&quot;/museum_photos/Bìa ảnh sections/Dòng chảy hiện vật.png&quot;);
            "
          ></div>
          <div class="exp-card-overlay"></div>
          <div class="exp-card-content">
            <h3>Dòng chảy hiện vật</h3>
            <p>
              Khám phá hành trình phát triển của lực lượng Phòng không - Không
              quân Việt Nam qua những hiện vật tiêu biểu của từng thời kỳ lịch
              sử.
            </p>
            <span class="btn-gold">KHÁM PHÁ →</span>
          </div>
        </router-link>

        <router-link to="/kham-pha/ky-uc" class="exp-card">
          <div
            class="exp-card-bg"
            style="
              background-image: url(&quot;/museum_photos/Bìa ảnh sections/Ký ức lịch sử.png&quot;);
            "
          ></div>
          <div class="exp-card-overlay"></div>
          <div class="exp-card-content">
            <h3>Ký ức lịch sử</h3>
            <p>
              Lắng nghe những câu chuyện về con người, sự kiện và dấu mốc lịch
              sử phía sau mỗi hiện vật và chiến công trên bầu trời Tổ quốc.
            </p>
            <span class="btn-gold">KHÁM PHÁ →</span>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<style scoped>
.explore-page {
  position: relative;
  min-height: 100vh;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 40px 20px;
  /* No overflow hidden – content can scroll naturally */
}

/* Background image – fixed to viewport, behind everything */
.explore-bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1; /* Behind all content */
  background-image: url("/museum_photos/Trang tham quan/Nền.png");
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  filter: blur(8px) brightness(0.7);
  transform: scale(1.05); /* hide blur edges */
}

/* Semi-transparent overlay – fixed, on top of background */
.explore-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 0;
  background: rgba(0, 0, 0, 0.3);
}

/* Content – above overlay, scrolls with page */
.explore-content {
  position: relative;
  z-index: 1;
  max-width: 1200px;
  width: 100%;
  text-align: center;
}

.page-title {
  font-family: var(--font-title);
  font-size: 48px;
  color: var(--white);
  text-shadow: 0 2px 12px rgba(0, 0, 0, 0.5);
  margin-bottom: 12px;
  letter-spacing: 2px;
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
}

.explore-cards {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 30px;
  margin-top: 20px;
}

.exp-card {
  position: relative;
  border-radius: var(--radius);
  overflow: hidden;
  height: 420px;
  text-decoration: none;
  color: var(--white);
  display: block;
  transition: var(--transition);
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.3);
}
.exp-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 16px 40px rgba(0, 0, 0, 0.4);
}
.exp-card-bg {
  position: absolute;
  inset: 0;
  background-size: cover;
  background-position: center;
}
.exp-card-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    transparent 0%,
    rgba(0, 0, 0, 0.2) 50%,
    rgba(0, 0, 0, 0.7) 100%
  );
}
.exp-card-content {
  position: relative;
  z-index: 2;
  padding: 30px;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}
.exp-card-content h3 {
  font-family: var(--font-title);
  font-size: 26px;
  letter-spacing: 0.5px;
  margin-bottom: 8px;
}
.exp-card-content p {
  font-family: var(--font-body);
  font-size: 15px;
  opacity: 0.9;
  margin-bottom: 16px;
  max-width: 90%;
  line-height: 1.6;
}
.exp-card-content .btn-gold {
  background: var(--gold);
  color: var(--primary-dark);
  padding: 8px 24px;
  border-radius: 30px;
  font-weight: 600;
  font-size: 14px;
  align-self: flex-start;
  transition: var(--transition);
  display: inline-block;
}
.exp-card-content .btn-gold:hover {
  background: var(--gold-light);
}

/* Responsive */
@media (max-width: 900px) {
  .explore-cards {
    grid-template-columns: 1fr 1fr;
    gap: 20px;
  }
  .page-title {
    font-size: 36px;
  }
  .page-subtitle {
    font-size: 16px;
    padding: 0 20px;
  }
}
@media (max-width: 600px) {
  .explore-cards {
    grid-template-columns: 1fr;
    gap: 20px;
  }
  .exp-card {
    height: 320px;
  }
  .page-title {
    font-size: 28px;
  }
  .explore-page {
    padding: 20px 12px;
  }
}
</style>
