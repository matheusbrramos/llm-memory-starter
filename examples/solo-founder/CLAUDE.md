# MeuProduto — Project Root

## O Que e Este Repositorio

SaaS B2C em fase de pre-lancamento. Este repo contem site, backend, docs e marketing.

---

## Mapa do Projeto

```
.
├── site/                    # Landing page + blog
│   └── CLAUDE.md            #   → Design system, deploy, regras visuais
│
├── backend/                 # API + servicos
│   └── CLAUDE.md            #   → Endpoints, migrations, autenticacao
│
├── docs/                    # Documentacao
│   ├── adr/                 #   → Architecture Decision Records
│   └── content/             #   → Blog posts, estrategia de conteudo
│
├── marketing/               # Growth engine
│   └── CLAUDE.md            #   → Agentes, campanhas, convencoes
│
├── .context/
│   └── empresa.md           #   → Missao, publico, diferenciais
│
└── CHANGELOG.md             # Historico de mudancas
```

**Regra: Sempre leia o CLAUDE.md da subpasta antes de trabalhar nela.**

---

## Estado Atual

| Componente | Versao | Status |
|------------|--------|--------|
| Site | v1.0 | Live |
| Backend | v0.8 | Beta fechado |
| Blog | — | 5 artigos publicados |

---

## Change Tracking

1. **CHANGELOG.md** — O que mudou
2. **ADR** (`docs/adr/`) — Por que mudou
3. **Git Tags** — Quando mudou

---

## Comportamento do Agente

### Inicio
1. Ler `.context/empresa.md`
2. Ler `CLAUDE.md` da subpasta relevante
3. Confirmar objetivo

### Final
1. Resumir entrega
2. Proximos passos
3. Perguntar se quer ajustar
