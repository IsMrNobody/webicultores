<template>
  <div class="tooltip-menu">
    <div class="welcome">Bienvenido</div>
    <div class="question">¿Hacia dónde quieres navegar?</div>
    <div class="options">
      <button @click="$emit('navigate', '/portfolio')">web</button>
      <button @click="$emit('navigate', '/branding')">design</button>
      <button @click="$emit('navigate', '/video')">video</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TooltipMenu',
}
</script>

<style scoped>
.tooltip-menu {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}
.welcome {
  font-weight: bold;
  font-size: 1.1em;
}
.question {
  margin-bottom: 4px;
}
.options button {
  margin: 2px 0;
  padding: 6px 16px;
  border-radius: 6px;
  border: none;
  background: #1976d2;
  color: #fff;
  font-size: 1em;
  cursor: pointer;
  transition: background 0.2s;
}
.options button:hover {
  background: #125ea2;
}

@media (max-width: 600px) {
  .tooltip-menu {
    gap: 6px;
  }
  .welcome {
    font-size: 1em;
  }
  .question {
    font-size: 0.95em;
  }
  .options button {
    padding: 4px 10px;
    font-size: 0.95em;
    border-radius: 4px;
  }
}
</style>
