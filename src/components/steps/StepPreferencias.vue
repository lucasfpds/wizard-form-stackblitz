<script setup>
defineProps({
  form: Object,
  errors: Object,
});
defineEmits(["validate"]);

const INTERESSES = ["Tech", "Design", "Música", "Esportes", "Viagem", "Cinema", "Gastronomia", "Livros"];
const FONTES = ["Indicação", "Redes sociais", "Blog", "Outro"];

function toggle(i) {
  const idx = form.interesses.indexOf(i);
  if (idx >= 0) form.interesses.splice(idx, 1);
  else form.interesses.push(i);
}
</script>

<template>
  <div class="step-content">
    <h2>Preferências</h2>
    <div class="field">
      <span class="field-label">Interesses (mín. 1)</span>
      <div class="chips">
        <button
          v-for="i in INTERESSES"
          :key="i"
          class="chip"
          :class="{ active: form.interesses.includes(i) }"
          @click="toggle(i); $emit('validate', 'interesses')"
        >{{ i }}</button>
      </div>
      <span v-if="errors.interesses" class="err">{{ errors.interesses }}</span>
    </div>
    <div class="field">
      <label for="wf-fonte">Como nos conheceu?</label>
      <select id="wf-fonte" v-model="form.fonte" @change="$emit('validate', 'fonte')" :class="{ error: errors.fonte }">
        <option value="">Selecione...</option>
        <option v-for="f in FONTES" :key="f" :value="f">{{ f }}</option>
      </select>
      <span v-if="errors.fonte" class="err">{{ errors.fonte }}</span>
    </div>
    <div v-if="form.fonte === 'Outro'" class="field">
      <label for="wf-fonte-outro">Especifique</label>
      <input
        id="wf-fonte-outro"
        v-model="form.fonteOutro"
        @blur="$emit('validate', 'fonteOutro')"
        :class="{ error: errors.fonteOutro }"
        placeholder="Como?"
      />
      <span v-if="errors.fonteOutro" class="err">{{ errors.fonteOutro }}</span>
    </div>
  </div>
</template>
