<template>
  <header class="site-header">
    <div class="container header-inner">
      <router-link to="/" class="logo-area">
        <img
          src="/logo.png"
          alt="Bảo tàng Phòng không - Không quân"
          class="logo-image"
        />
        <div class="logo-text">BẢO TÀNG PHÒNG KHÔNG - KHÔNG QUÂN</div>
      </router-link>
      <nav class="main-nav">
        <router-link to="/">Trang chủ</router-link>

        <div
          class="dropdown"
          @mouseenter="openExplore"
          @mouseleave="closeExplore"
        >
          <router-link to="/kham-pha" class="dropdown-trigger"
            >Khám phá</router-link
          >
          <div class="dropdown-menu" v-show="exploreOpen">
            <router-link to="/kham-pha/hien-vat"
              >Dòng chảy hiện vật</router-link
            >
            <router-link to="/kham-pha/ky-uc">Ký ức lịch sử</router-link>
          </div>
        </div>

        <div
          class="dropdown"
          @mouseenter="openExperience"
          @mouseleave="closeExperience"
        >
          <router-link to="/trai-nghiem" class="dropdown-trigger"
            >Trải nghiệm</router-link
          >
          <div class="dropdown-menu" v-show="experienceOpen">
            <router-link to="/trai-nghiem/genz">Góc nhìn GenZ</router-link>
            <router-link to="/trai-nghiem/puzzle"
              >Truy tìm mảnh ghép</router-link
            >
          </div>
        </div>

        <router-link to="/tham-quan">Tham quan</router-link>
      </nav>
    </div>
  </header>
</template>

<script setup>
import { ref } from "vue";

const exploreOpen = ref(false);
const experienceOpen = ref(false);

function openExplore() {
  exploreOpen.value = true;
}
function closeExplore() {
  exploreOpen.value = false;
}
function openExperience() {
  experienceOpen.value = true;
}
function closeExperience() {
  experienceOpen.value = false;
}
</script>

<style scoped>
.site-header {
  background: var(--primary-dark);
  color: var(--white);
  padding: 12px 0;
  position: sticky;
  top: 0;
  z-index: 1000;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.3);
  font-family: var(--font-header-footer);
}
.header-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 12px;
}

.logo-area {
  display: flex;
  align-items: center;
  gap: 12px;
  text-decoration: none;
  color: inherit;
  cursor: pointer;
}
.logo-image {
  height: 48px;
  width: auto;
  display: block;
}
.logo-text {
  font-weight: 700;
  font-size: 18px;
  line-height: 1.2;
}
.logo-text small {
  display: block;
  font-weight: 400;
  font-size: 12px;
  opacity: 0.8;
}

.main-nav {
  display: flex;
  gap: 6px;
  flex-wrap: wrap;
  align-items: center;
}
.main-nav a {
  padding: 8px 16px;
  border-radius: 4px;
  font-size: 14px;
  font-weight: 500;
  transition: var(--transition);
  border: 1px solid transparent;
  text-decoration: none;
  color: var(--white);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  white-space: nowrap;
}
.main-nav a:hover,
.main-nav a.router-link-active {
  background: #fada2d;
  color: var(--primary-dark);
  border-color: #fada2d;
}

.dropdown {
  position: relative;
  display: inline-block;
}
.dropdown-trigger {
  cursor: default;
}
.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  background: var(--white);
  border-radius: 4px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.2);
  min-width: 160px;
  padding: 6px 0;
  z-index: 2000;
}
.dropdown-menu a {
  display: block;
  padding: 8px 20px;
  color: var(--primary-dark);
  font-size: 13px;
  font-weight: 500;
  border: none;
  border-radius: 0;
  text-transform: uppercase;
  letter-spacing: 0.3px;
  white-space: nowrap;
}
.dropdown-menu a:hover {
  background: var(--cream);
}

@media (max-width: 768px) {
  .header-inner {
    flex-direction: column;
    align-items: stretch;
  }
  .main-nav {
    justify-content: center;
  }
  .main-nav a {
    font-size: 12px;
    padding: 6px 10px;
  }
  .dropdown-menu a {
    font-size: 12px;
    padding: 6px 16px;
  }
}
@media (max-width: 480px) {
  .main-nav a {
    font-size: 11px;
    padding: 4px 8px;
  }
  .logo-text {
    font-size: 15px;
  }
  .logo-image {
    height: 36px;
  }
  .dropdown-menu a {
    font-size: 11px;
    padding: 4px 12px;
  }
}
</style>
