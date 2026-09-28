<script setup>
defineProps({
  form: Object,
  errors: Object,
});
defineEmits(["validate"]);
</script>

<template>
  <div class="step-content">
    <h2>Conta</h2>
    <div class="field">
      <label for="wf-nome">Nome</label>
      <input
        id="wf-nome"
        :value="form.nome"
        @input="form.nome = $event.target.value"
        @blur="$emit('validate', 'nome')"
        :class="{ error: errors.nome }"
        placeholder="Seu nome"
      />
      <span v-if="errors.nome" class="err">{{ errors.nome }}</span>
    </div>
    <div class="field">
      <label for="wf-email">E-mail</label>
      <input
        id="wf-email"
        :value="form.email"
        @input="form.email = $event.target.value"
        @blur="$emit('validate', 'email')"
        :class="{ error: errors.email }"
        placeholder="voe@email.com"
      />
      <span v-if="errors.email" class="err">{{ errors.email }}</span>
    </div>
    <div class="field">
      <label for="wf-senha">Senha (mín. 8)</label>
      <input
        id="wf-senha"
        type="password"
        :value="form.senha"
        @input="form.senha = $event.target.value"
        @blur="$emit('validate', 'senha')"
        :class="{ error: errors.senha }"
        placeholder="********"
      />
      <span v-if="errors.senha" class="err">{{ errors.senha }}</span>
    </div>
  </div>
</template>
