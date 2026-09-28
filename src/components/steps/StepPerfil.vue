<script setup>
defineProps({
  form: Object,
  errors: Object,
});
defineEmits(["validate"]);
</script>

<template>
  <div class="step-content">
    <h2>Perfil</h2>
    <div class="field">
      <span class="field-label">Tipo</span>
      <div class="radios">
        <label class="radio">
          <input type="radio" value="PF" v-model="form.tipo" @change="$emit('validate', 'doc')" />
          Pessoa Física
        </label>
        <label class="radio">
          <input type="radio" value="PJ" v-model="form.tipo" @change="$emit('validate', 'doc')" />
          Pessoa Jurídica
        </label>
      </div>
    </div>
    <div class="field">
      <label for="wf-doc">{{ form.tipo === 'PF' ? 'CPF (11 dígitos)' : 'CNPJ (14 dígitos)' }}</label>
      <input
        id="wf-doc"
        :value="form.doc"
        @input="form.doc = $event.target.value.replace(/\D/g, '')"
        @blur="$emit('validate', 'doc')"
        :class="{ error: errors.doc }"
        :maxlength="form.tipo === 'PF' ? 11 : 14"
        :placeholder="form.tipo === 'PF' ? '00000000000' : '00000000000000'"
      />
      <span v-if="errors.doc" class="err">{{ errors.doc }}</span>
    </div>
  </div>
</template>
