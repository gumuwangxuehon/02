---
title: 壁纸
date: 2026-08-17 16:00:00
type: wallpapers
top_img: /img/avatar.png
comments: false
---

<style>
.wallpaper-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
  margin: 20px 0;
}

.wallpaper-card {
  position: relative;
  overflow: hidden;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.12);
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  background: var(--card-bg, #fff);
}

.wallpaper-card:hover {
  transform: translateY(-6px);
  box-shadow: 0 10px 24px rgba(0, 0, 0, 0.2);
}

.wallpaper-card img {
  width: 100%;
  height: 220px;
  object-fit: cover;
  display: block;
  cursor: pointer;
  transition: transform 0.4s ease;
}

.wallpaper-card:hover img {
  transform: scale(1.05);
}

.wallpaper-card .caption {
  padding: 10px 14px;
  font-size: 14px;
  color: var(--font-color, #333);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.wallpaper-card .download-btn {
  font-size: 13px;
  padding: 4px 10px;
  border-radius: 6px;
  background: var(--btn-bg, #425aef);
  color: #fff;
  text-decoration: none;
  transition: background 0.2s;
}

.wallpaper-card .download-btn:hover {
  background: var(--btn-hover-bg, #1f3fd1);
}

.wallpaper-lightbox {
  display: none;
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.88);
  z-index: 9999;
  justify-content: center;
  align-items: center;
  cursor: zoom-out;
}

.wallpaper-lightbox.active {
  display: flex;
}

.wallpaper-lightbox img {
  max-width: 92%;
  max-height: 90vh;
  border-radius: 8px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
}

.wallpaper-tip {
  text-align: center;
  color: var(--font-color, #666);
  font-size: 14px;
  margin: 10px 0 20px;
}
</style>

> 点击图片可放大预览，点击「下载」按钮可保存原图。

<div class="wallpaper-grid" id="wallpaperGrid">
  <div class="wallpaper-card">
    <img src="/wallpapers/wallpaper-1.png" alt="达令！！" loading="lazy" onclick="openLightbox(this.src)">
    <div class="caption">
      <span>达令！！</span>
      <a class="download-btn" href="/wallpapers/wallpaper-1.png" download>下载</a>
    </div>
  </div>

  <div class="wallpaper-card">
    <img src="/wallpapers/wallpaper-2.png" alt="Darling in the Franxx" loading="lazy" onclick="openLightbox(this.src)">
    <div class="caption">
      <span>Darling in the Franxx</span>
      <a class="download-btn" href="/wallpapers/wallpaper-2.png" download>下载</a>
    </div>
  </div>

  <div class="wallpaper-card">
    <img src="/wallpapers/wallpaper-3.png" alt="4K 高清" loading="lazy" onclick="openLightbox(this.src)">
    <div class="caption">
      <span>达令！！</span>
      <a class="download-btn" href="/wallpapers/wallpaper-3.png" download>下载</a>
    </div>
  </div>

  <div class="wallpaper-card">
    <img src="/wallpapers/wallpaper-4.png" alt="国家队" loading="lazy" onclick="openLightbox(this.src)">
    <div class="caption">
      <span>02</span>
      <a class="download-btn" href="/wallpapers/wallpaper-4.png" download>下载</a>
    </div>
  </div>

  <div class="wallpaper-card">
    <img src="/wallpapers/wallpaper-5.png" alt="动漫合集" loading="lazy" onclick="openLightbox(this.src)">
    <div class="caption">
      <span>02</span>
      <a class="download-btn" href="/wallpapers/wallpaper-5.png" download>下载</a>
    </div>
  </div>
</div>

<div class="wallpaper-lightbox" id="wallpaperLightbox" onclick="closeLightbox()">
  <img id="lightboxImage" src="" alt="预览">
</div>

<script>
function openLightbox(src) {
  var lb = document.getElementById('wallpaperLightbox');
  document.getElementById('lightboxImage').src = src;
  lb.classList.add('active');
}

function closeLightbox() {
  document.getElementById('wallpaperLightbox').classList.remove('active');
}

document.addEventListener('keydown', function (e) {
  if (e.key === 'Escape') closeLightbox();
});
</script>
