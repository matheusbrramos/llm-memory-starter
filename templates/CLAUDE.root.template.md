# {{PROJECT_NAME}} — Project Root

<!--
  CLAUDE.md (raiz) — O "router" do seu projeto.
  
  Este arquivo e o PRIMEIRO que o agente le quando abre o projeto.
  Ele responde: "O que e isso? O que tem aqui? Por onde comeco?"
  
  Regras:
  1. NUNCA duplique informacao que ja esta num CLAUDE.md de subpasta
  2. Este arquivo e um MAPA, nao um manual
  3. Mantenha atualizado — informacao velha aqui e pior que informacao nenhuma
-->

## O Que e Este Repositorio

{{DESCRICAO_EM_2_FRASES}}

---

## Mapa do Projeto

<!--
  Uma arvore visual das pastas principais.
  Pra cada pasta, diga o que tem e se tem CLAUDE.md proprio.
  Use comentarios inline (→) pra guiar o agente. -->

```
.
├── {{PASTA_1}}/             # {{O_QUE_FAZ}}
│   ├── CLAUDE.md            #   → Regras especificas desta area
│   └── ...
│
├── {{PASTA_2}}/             # {{O_QUE_FAZ}}
│   └── ...
│
├── docs/                    # Documentacao
│   ├── adr/                 #   → Architecture Decision Records
│   └── ...
│
├── .context/                # Contexto persistente
│   └── empresa.md           #   → Missao, produtos, publico
│
└── CHANGELOG.md             # Historico de mudancas
```

**Regra: Sempre leia o `CLAUDE.md` da subpasta antes de trabalhar nela.**

---

## Estado Atual

<!-- Snapshot do estado do projeto. Mantenha atualizado. -->

| Componente | Versao | Status |
|------------|--------|--------|
| {{COMPONENTE_1}} | {{VERSAO}} | {{STATUS}} |
| {{COMPONENTE_2}} | {{VERSAO}} | {{STATUS}} |

---

## Change Tracking

<!--
  Tres niveis de registro obrigatorios.
  Isso garante que o agente de amanha entende o que o agente de hoje fez. -->

1. **CHANGELOG.md** — O que mudou. Formato Keep a Changelog.
2. **ADR** (`docs/adr/`) — Por que mudou. Nunca deletar, marcar Deprecated.
3. **Git Tags** — Quando mudou. `vMAJOR.MINOR.PATCH` (SemVer).

```
Fluxo: CHANGELOG → ADR (se arquitetural) → commit → tag → push
```

---

## Comportamento do Agente

### Inicio de sessao
1. Verificar `.context/empresa.md` (contexto da empresa/projeto)
2. Ler `CLAUDE.md` da subpasta relevante
3. Confirmar objetivo com o usuario

### Final de sessao
1. Resumir entrega
2. Proximos passos recomendados
3. Perguntar se o usuario quer ajustar
