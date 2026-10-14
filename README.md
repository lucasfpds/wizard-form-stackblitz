# Formulário Wizard com Etapas Dinâmicas

> Múltiplas etapas, validação condicional e transições de página fluídas com indicador de progresso.

## Tecnologias

- **Vue 3** com `<script setup>` (JavaScript)
- **Vite** como bundler
- **CSS puro** com transições direcionais (`translateX`)
- **Transition** do Vue com nome dinâmico para avançar/voltar
- Validação condicional sem libs (regras por campo)

## Requisitos

- Node.js 18+

## Como executar

```bash
npm install
npm run dev
```

Abra http://localhost:5173 no navegador.

## Scripts

| Script | Descrição |
| --- | --- |
| `npm run dev` | Servidor de desenvolvimento (Vite) |
| `npm run build` | Build de produção |
| `npm run preview` | Serve o build de produção |

## Estrutura

```
src/
  main.js              entry: cria o app e importa o CSS
  App.vue              estado do form, etapas, validação, transições
  components/
    StepIndicator.vue  círculos numerados + trilha preenchida
    steps/
      StepConta.vue        nome, e-mail, senha
      StepPerfil.vue       PF/PJ + CPF/CNPJ condicional
      StepPreferencias.vue interesses + fonte (com "Outro")
      StepRevisao.vue      resumo + editar por etapa
  styles.css           indicador, campos, chips, transições, sucesso
```

### Funcionalidades

- 4 etapas: Conta, Perfil, Preferências, Revisão
- Validação por campo: required, e-mail regex, senha ≥ 8, dígitos de documento
- Valida ao avançar (bloqueia se inválida) e on blur
- Validação condicional: PF → CPF (11 dígitos), PJ → CNPJ (14 dígitos); "Outro" no select adiciona campo obrigatório
- Voltar é sempre livre; cada etapa mantém o que foi preenchido (estado único no App.vue)
- Indicador de progresso: círculos numerados com estados concluído (✓ verde) / ativo / pendente + trilha preenchida
- Transições de página com direção: avançar — nova etapa entra da direita, antiga sai para esquerda; voltar inverte (0.35s ease)
- Revisão com botão "editar" por etapa
- Envio: loading curto → tela de sucesso com check animado


## Rodando no StackBlitz

Basta importar o repositório e rodar normalmente:

```
https://stackblitz.com/github/lucasfpds/wizard-form-stackblitz
```

O projeto não depende de binários nativos — Vite é JavaScript puro, então
sobe sem bindings nativos no WebContainer do StackBlitz.
