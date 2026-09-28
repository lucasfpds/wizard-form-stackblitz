<script setup>
defineProps({
  total: Number,
  current: Number,
});

const labels = ["Conta", "Perfil", "Preferências", "Revisão"];
</script>

<template>
  <div class="indicator">
    <div
      v-for="i in total"
      :key="i"
      class="step"
      :class="{
        'step--done': i - 1 < current,
        'step--active': i - 1 === current,
      }"
    >
      <div class="circle">
        <span v-if="i - 1 < current">✓</span>
        <span v-else>{{ i }}</span>
      </div>
      <span class="label">{{ labels[i - 1] }}</span>
      <div v-if="i < total" class="trail" :class="{ 'trail--filled': i - 1 < current }"></div>
    </div>
  </div>
</template>
