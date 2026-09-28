# Formulário Wizard com Etapas Dinâmicas

> Múltiplas etapas, validação condicional e transições de página fluídas com indicador de progresso.

## Stack

- Vite + Vue 3 (`<script setup>`, JavaScript) — sem TypeScript, sem lint/test, zero libs extras
- CSS puro em `src/styles.css`
- Arquivos: `index.html`, `package.json` (deps: `vue`), `vite.config.js`, `.stackblitzrc`, `src/main.js`, `src/App.vue`, `src/components/StepIndicator.vue`, `src/components/steps/StepConta.vue`, `src/components/steps/StepPerfil.vue`, `src/components/steps/StepPreferencias.vue`, `src/components/steps/StepRevisao.vue`, `src/styles.css`

## Implementação

### 1. Etapas

1. **Conta**: nome, e-mail, senha (≥ 8)
2. **Perfil**: tipo PF/PJ (radio) → mostra campo **CPF (11 dígitos)** ou **CNPJ (14 dígitos)** conforme a escolha
3. **Preferências**: interesses (chips, mín. 1) + "como nos conheceu?" (select; se "Outro", campo texto obrigatório)
4. **Revisão**: resumo de tudo com botão "editar" por etapa → Enviar → tela de sucesso com check animado

### 2. Validação condicional

- Regras por campo: required, e-mail regex, senha ≥ 8, dígitos de documento; erro inline abaixo do campo + borda vermelha
- Valida ao avançar (bloqueia se inválida) e on blur
- **Condicional**: o validador da etapa considera apenas os campos visíveis — trocar PF ↔ PJ troca campos e regras; "Outro" no select adiciona campo obrigatório
- Voltar é sempre livre; cada etapa mantém o que foi preenchido (estado único no App.vue)

### 3. Indicador de progresso

- `components/StepIndicator.vue`: círculos numerados com estados concluído (✓ verde) / ativo / pendente + trilha preenchida proporcional à etapa

### 4. Transições de página

- `<Transition>` com direção: avançar — a nova etapa entra da direita (`translateX(100%) → 0`) e a antiga sai para a esquerda; voltar inverte; 0.35s ease
- Envio: loading curto → sucesso

## Checklist — 100% da descrição

- [ ] Múltiplas etapas com navegação (voltar/avançar)
- [ ] Validação por etapa + campos e regras condicionais
- [ ] Transições de página fluidas e direcionais
- [ ] Indicador de progresso (stepper + trilha)
