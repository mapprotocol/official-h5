<template>
  <section class="product-media" :class="'product-media-'+ $i18n.locale">
    <div class="ctx-main">
      <div class="map-title"><p>Marcopolo Protocol Introduction</p></div>
      <div class="video_box">
        <video width="320" poster="https://files.marcopolo.link/image/product-media.png" controls controlslist="nodownload" style="object-fit: fill; width: 100%; height: 100%">
          <source src="https://files.marcopolo.link/video/marcopolo.mp4">
          <source src="https://files.marcopolo.link/video/marcopolo.ogg">
          <p>Your browser does not support the HTML5 video tag.</p>
        </video>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  name: 'ProductMedia.vue'
}
</script>

<style scoped>

</style>
