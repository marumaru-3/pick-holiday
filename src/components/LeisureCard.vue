<script setup>
defineProps({
  name: {
    type: String,
    required: true
  },
  imgSrc: {
    type: String,
    required: true
  }
})
</script>
<template>
  <div class="card">
    <div>{{ name }}</div>
    <div class="card-img">
      <img :src="imgSrc" alt="" />
    </div>
  </div>
</template>
<style scoped>
.card {
  box-shadow: 2px 2px 6px rgba(0, 0, 0, 0.4);
  border-radius: 5px;
  font-size: 24px;
  padding: 16px;
  cursor: pointer;
  width: 400px;
  height: 400px;
  transition: .3s all;
}
.card:hover {
  transform: translateY(-8px);
  box-shadow: 2px 4px 8px rgba(0, 0, 0, 0.4);
}
.selected-end .card {
  cursor: auto;
}
.selected-end .card:hover {
  transform: none;
  box-shadow: 2px 2px 6px rgba(0, 0, 0, 0.4);
}

.card-img {
  width: 90%;
  height: 90%;
  margin: auto;
  padding: 10px;
  display: grid;
  place-items: center;
}
@media (max-width: 899px) {
  .card {
    width: 300px;
    height: 300px;
    font-size: 20px;
  }
}
</style>
