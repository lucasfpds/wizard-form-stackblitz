<script setup>
import { ref, computed } from "vue";
import StepIndicator from "./components/StepIndicator.vue";
import StepConta from "./components/steps/StepConta.vue";
import StepPerfil from "./components/steps/StepPerfil.vue";
import StepPreferencias from "./components/steps/StepPreferencias.vue";
import StepRevisao from "./components/steps/StepRevisao.vue";

const step = ref(0);
const direction = ref("next");
const submitting = ref(false);
const done = ref(false);

const form = ref({
  nome: "",
  email: "",
  senha: "",
  tipo: "PF",
  doc: "",
  interesses: [],
  fonte: "",
  fonteOutro: "",
});

const errors = ref({});

const TOTAL = 4;

function validateField(field) {
  const f = form.value;
  const errs = {};
  if (field === "nome" || field === null) errs.nome = !f.nome ? "Obrigatório" : "";
  if (field === "email" || field === null) {
    errs.email = !f.email ? "Obrigatório" : !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(f.email) ? "E-mail inválido" : "";
  }
  if (field === "senha" || field === null) errs.senha = f.senha.length < 8 ? "Mínimo 8 caracteres" : "";
  if (field === "doc" || field === null) {
    const len = f.tipo === "PF" ? 11 : 14;
    errs.doc = f.doc.length !== len ? `Deve ter ${len} dígitos` : "";
  }
  if (field === "interesses" || field === null) errs.interesses = f.interesses.length < 1 ? "Mínimo 1 interesse" : "";
  if (field === "fonte" || field === null) errs.fonte = !f.fonte ? "Obrigatório" : "";
  if (field === "fonteOutro" || field === null) errs.fonteOutro = f.fonte === "Outro" && !f.fonteOutro ? "Obrigatório" : "";

  if (field) {
    errors.value = { ...errors.value, [field]: errs[field] };
  } else {
    errors.value = errs;
  }
  return Object.values(errs).every((e) => !e);
}

function validateStep() {
  if (step.value === 0) {
    return ["nome", "email", "senha"].every((f) => validateField(f));
  }
  if (step.value === 1) return validateField("doc");
  if (step.value === 2) {
    return ["interesses", "fonte", "fonteOutro"].every((f) => validateField(f));
  }
  return true;
}

function next() {
  if (!validateStep()) return;
  if (step.value < TOTAL - 1) {
    direction.value = "next";
    step.value++;
  } else {
    submit();
  }
}

function prev() {
  if (step.value > 0) {
    direction.value = "prev";
    step.value--;
  }
}

function editStep(i) {
  direction.value = i > step.value ? "next" : "prev";
  step.value = i;
}

function submit() {
  submitting.value = true;
  setTimeout(() => {
    submitting.value = false;
    done.value = true;
  }, 1000);
}

function restart() {
  done.value = false;
  step.value = 0;
  form.value = { nome: "", email: "", senha: "", tipo: "PF", doc: "", interesses: [], fonte: "", fonteOutro: "" };
  errors.value = {};
}

const transitionName = computed(() => "slide-" + direction.value);
</script>

<template>
  <main class="app">
    <h1>Formulário Wizard</h1>
    <StepIndicator :total="TOTAL" :current="step" v-if="!done" />
    <Transition :name="transitionName" mode="out-in">
      <div v-if="done" class="success" key="done">
        <div class="check">✓</div>
        <h2>Cadastro concluído!</h2>
        <button @click="restart">Novo cadastro</button>
      </div>
      <div v-else key="form" class="form-wrap">
        <StepConta v-if="step === 0" :form="form" :errors="errors" @validate="validateField" />
        <StepPerfil v-else-if="step === 1" :form="form" :errors="errors" @validate="validateField" />
        <StepPreferencias v-else-if="step === 2" :form="form" :errors="errors" @validate="validateField" />
        <StepRevisao v-else :form="form" @edit="editStep" />

        <div class="nav">
          <button v-if="step > 0" @click="prev" class="btn-sec">Voltar</button>
          <button @click="next" :disabled="submitting" class="btn-primary">
            {{ submitting ? "Enviando..." : step === TOTAL - 1 ? "Enviar" : "Avançar" }}
          </button>
        </div>
      </div>
    </Transition>
  </main>
</template>
