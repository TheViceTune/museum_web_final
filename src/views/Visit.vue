<template>
  <div class="visit-page">
    <!-- Full‑page background -->
    <div class="page-bg"></div>
    <div class="page-overlay"></div>

    <!-- Content -->
    <div class="container page">
      <h1 class="page-title" v-slide-in:left>Tham quan</h1>
      <p class="page-subtitle" v-slide-in:left="{ delay: 100 }">
        Giờ mở cửa · Vé vào cổng · Địa chỉ &amp; lưu ý
      </p>

      <div class="visit-grid">
        <div class="visit-card" v-slide-in:bottom="{ delay: 0 }">
          <h3>🕐 Giờ mở cửa</h3>
          <ul>
            <li>
              <strong>Thứ 2 – Thứ 5</strong><br />08:00 – 11:00 &amp; 13:00 –
              16:00
            </li>
            <li>
              <strong>Thứ 7, Chủ nhật</strong><br />Phục vụ khách lẻ tham quan
              ngoài trời từ 16:00 – 18:30
            </li>
            <li style="color: #c62828; font-weight: 600">Thứ 6: ĐÓNG CỬA</li>
          </ul>
        </div>

        <div class="visit-card" v-slide-in:bottom="{ delay: 100 }">
          <h3>🎫 Vé vào cổng</h3>
          <ul>
            <li>
              <span class="highlight">Người lớn (khách Việt Nam):</span> 10.000
              – 15.000 VND
            </li>
            <li>
              <span class="highlight">Người lớn (khách nước ngoài):</span>
              30.000 VND
            </li>
            <li>
              <span class="highlight">Trẻ em trên 6 tuổi:</span> 5.000 – 10.000
              VND
            </li>
            <li><span class="highlight">Trẻ em dưới 6 tuổi:</span> Miễn phí</li>
          </ul>
        </div>

        <div class="visit-card" v-slide-in:bottom="{ delay: 200 }">
          <h3>📍 Địa chỉ &amp; Liên hệ</h3>
          <ul>
            <li>
              <strong>Địa chỉ:</strong> Số 173C đường Trường Chinh, Phường
              Khương Mai, TP. Hà Nội
            </li>
            <li><strong>Điện thoại:</strong> 02435 632 686</li>
            <li>
              <strong>Phương tiện:</strong> Xe máy, ô tô (có chỗ để xe), xe buýt
              (tuyến 16, 19, 24), taxi/xe công nghệ
            </li>
          </ul>
        </div>

        <div class="visit-card" v-slide-in:bottom="{ delay: 300 }">
          <h3>📋 Lưu ý</h3>
          <ul>
            <li>✓ Giữ trật tự, không nói tục, gây ồn</li>
            <li>✓ Không leo trèo, chạm, khắc vẽ lên hiện vật</li>
            <li>
              ✓ Tránh đi ngày mưa để tham quan trọn vẹn không gian ngoài trời
            </li>
            <li>✓ Mang theo mũ, ô và nước uống</li>
            <li>✓ Ăn mặc lịch sự, trang nhã</li>
            <li>✓ Cấm mang chất cháy nổ, vật sắc nhọn</li>
            <li>✓ Bỏ rác đúng nơi quy định</li>
          </ul>
        </div>
      </div>

      <div class="contact-card" v-slide-in:bottom="{ delay: 400 }">
        <h3>📬 Thông tin liên hệ</h3>
        <div class="contact-grid">
          <div>
            <strong>Điện thoại quân sự:</strong> 069.562.323 · 069.562.322
          </div>
          <div><strong>Email:</strong> btpkkq171@gmail.com</div>
          <div><strong>Facebook:</strong>Bảo tàng Phòng không - Không quân</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// No additional imports needed – all styling is scoped
</script>

<style scoped>
/* Full‑page background */
.visit-page {
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
  background-image: url("/museum_photos/Trang tham quan/Nền.png");
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

/* Page title and subtitle – white with text shadow */
.page-title {
  font-size: 42px;
  color: var(--white);
  text-shadow: 0 2px 12px rgba(0, 0, 0, 0.5);
  margin-bottom: 8px;
}
.page-subtitle {
  font-size: 16px;
  color: var(--white);
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.4);
  margin-bottom: 30px;
}

/* Cards – keep original styles but add slight background opacity for readability */
.visit-card {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(4px);
  border-radius: var(--radius);
  padding: 24px;
  box-shadow: var(--shadow);
}
.visit-card h3 {
  color: var(--primary-dark);
  margin-bottom: 8px;
  font-size: 18px;
  border-bottom: 2px solid var(--gold);
  padding-bottom: 8px;
  font-family: var(--font-title);
  letter-spacing: 0.5px;
}
.visit-card ul {
  list-style: none;
  margin-top: 8px;
}
.visit-card ul li {
  padding: 4px 0;
  font-size: 14px;
  border-bottom: 1px solid #f0f0f0;
}
.visit-card ul li:last-child {
  border-bottom: none;
}
.visit-card .highlight {
  color: var(--primary-dark);
  font-weight: 600;
}

.contact-card {
  background: rgba(255, 255, 255, 0.9);
  backdrop-filter: blur(4px);
  border-radius: var(--radius);
  padding: 24px;
  box-shadow: var(--shadow);
  margin-top: 20px;
}
.contact-card h3 {
  color: var(--primary-dark);
  font-family: var(--font-title);
  letter-spacing: 0.5px;
}
.contact-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 12px;
  margin-top: 8px;
}

.visit-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(260px, 1fr));
  gap: 24px;
  margin: 20px 0;
}

/* Mobile responsiveness */
@media (max-width: 600px) {
  .visit-grid {
    grid-template-columns: 1fr;
  }
  .visit-card {
    padding: 16px;
  }
  .contact-grid {
    grid-template-columns: 1fr;
  }
  .page-title {
    font-size: 28px;
  }
}
</style>
