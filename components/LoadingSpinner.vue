<template>
  <div class="spinner-backdrop">
    <div class="spinner"></div>
    <div class="loading-text">Cargando modelo 3D...</div>
  </div>
</template>

<script>
export default {
  name: 'LoadingSpinner',
}
</script>

<style scoped>
.spinner-backdrop {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0,0,0,0.45);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  z-index: 20000;
}
.spinner {
  border: 5px solid #eee;
  border-top: 5px solid #1976d2;
  border-radius: 50%;
  width: 60px;
  height: 60px;
  animation: spin 1s linear infinite;
  margin-bottom: 16px;
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.loading-text {
  color: #fff;
  font-size: 1.1em;
  font-weight: 500;
  text-shadow: 0 2px 8px #000a;
}
</style>
