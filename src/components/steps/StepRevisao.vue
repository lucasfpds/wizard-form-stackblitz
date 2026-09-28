<script setup>
defineProps({
  form: Object,
});
defineEmits(["edit"]);
</script>

<template>
  <div class="step-content">
    <h2>Revisão</h2>
    <div class="review-block">
      <div class="review-head">
        <strong>Conta</strong>
        <button @click="$emit('edit', 0)">Editar</button>
      </div>
      <p>Nome: {{ form.nome }}</p>
      <p>E-mail: {{ form.email }}</p>
    </div>
    <div class="review-block">
      <div class="review-head">
        <strong>Perfil</strong>
        <button @click="$emit('edit', 1)">Editar</button>
      </div>
      <p>Tipo: {{ form.tipo }}</p>
      <p>Documento: {{ form.doc }}</p>
    </div>
    <div class="review-block">
      <div class="review-head">
        <strong>Preferências</strong>
        <button @click="$emit('edit', 2)">Editar</button>
      </div>
      <p>Interesses: {{ form.interesses.join(', ') }}</p>
      <p>Fonte: {{ form.fonte }}{{ form.fonte === 'Outro' ? ' — ' + form.fonteOutro : '' }}</p>
    </div>
  </div>
</template>
