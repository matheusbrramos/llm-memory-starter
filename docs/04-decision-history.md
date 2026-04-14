# Camada 4 — Historico de decisoes

*Por que fazemos as coisas de certo jeito.*

---

## O problema

Voce muda algo no projeto. Migra de banco de dados. Troca a arquitetura de autenticacao. Muda o fluxo de deploy.

Tres meses depois, o agente (ou voce mesmo) olha pro codigo e pergunta: "por que isso e assim?" Se a resposta so existe na cabeca de quem decidiu, ela ja se perdeu.

O agente de amanha precisa entender o que o agente de hoje fez — e mais importante, **por que**.

## A analogia

Pensa numa ata de reuniao. Uma ata ruim registra: "decidimos usar PostgreSQL." Uma ata boa registra: "decidimos usar PostgreSQL porque o DynamoDB nao permitia queries ad-hoc pra analytics, e o custo de manter dois bancos nao se justificava na fase atual."

A diferenca entre "o que" e "por que" e a diferenca entre documentacao que ajuda e documentacao que ocupa espaco.

## Os tres niveis de registro

Cada mudanca significativa gera ate tres registros, cada um com um proposito:

### 1. CHANGELOG.md — O que mudou

Formato: [Keep a Changelog](https://keepachangelog.com/). Simples, linear, humano.

```markdown
## [1.2.0] - 2026-04-10

### Added
- Sistema de memoria persistente com 4 tipos (user, feedback, project, reference)

### Changed
- Migrado de DynamoDB single-table para PostgreSQL (ver ADR-001)

### Fixed
- Corrigido bug onde memorias expiradas nao eram removidas do indice
```

O agente le isso pra entender **a evolucao recente** do projeto. E como um log de bordo.

### 2. ADRs (Architecture Decision Records) — Por que mudou

Formato: um arquivo por decisao, nunca deletado.

```markdown
# ADR-001: Migracao DynamoDB → PostgreSQL

**Date:** 2026-03-15
**Status:** Accepted

## Context
Precisavamos de queries ad-hoc pra analytics. DynamoDB nao suporta SQL.

## Decision
Migrar pra PostgreSQL hospedado em plataforma gerenciada (ex: Railway, Render, Supabase).

## Alternatives Considered
- Manter DynamoDB + adicionar Athena pra analytics: complexidade operacional alta
- MongoDB: sem vantagem clara sobre Postgres pro nosso caso

## Consequences
### Positive
- SQL ad-hoc pra analytics
- Modelo relacional mais intuitivo pra novos contributors

### Negative
- Custo fixo (instancia Postgres vs pay-per-request DynamoDB)

### Risks
- Se escalar muito rapido, connection pooling vira necessidade
```

**Regra de ouro dos ADRs: nunca deletar.** Uma decisao superseded e marcada como tal, apontando pra decisao nova. O historico e sagrado.

### 3. Git tags — Quando mudou

Tags semanticas (`v1.2.0`) marcam o momento exato. Permitem ao agente correlacionar codigo com decisoes: "essa mudanca entrou na v1.2.0, que foi quando migramos o banco (ADR-001)."

## O fluxo completo

```
Mudanca significativa
    ↓
CHANGELOG.md (o que mudou)
    ↓
ADR (se decisao arquitetural — por que mudou)
    ↓
Commit + Tag (quando mudou)
```

Nem toda mudanca precisa de ADR. Mas toda mudanca significativa precisa de CHANGELOG, e toda decisao arquitetural precisa de ADR.

## Por que isso importa pra agentes de IA

Um agente sem historico de decisoes so entende o estado ATUAL do codigo. Ele nao sabe:
- Que uma abordagem ja foi tentada e descartada (e pode sugerir de novo)
- Que uma decisao tem restricao de compliance (e pode propor algo que viola)
- Que uma migracao esta em curso (e pode sugerir coisas pro sistema antigo)

Com ADRs no repositorio, o agente tem acesso ao **contexto retroativo**: nao so o que existe, mas o que ja existiu e por que mudou.

## O que diferencia essa pratica

ADRs e changelogs existem ha decadas em engenharia de software. O que e menos comum:

1. **Tratar ADRs como contexto pra agentes, nao so documentacao pra humanos.** A maioria das equipes escreve ADRs pra onboarding de pessoas. Nos escrevemos pra onboarding de agentes tambem — que acontece toda sessao.

2. **Tres niveis integrados.** CHANGELOG (o que), ADR (por que), tags (quando). A maioria faz um ou dois. Os tres juntos criam uma malha navegavel.

## Como implementar

1. Crie `docs/adr/` e comece com o template em `/docs/adr-template.md`
2. Crie `CHANGELOG.md` na raiz. Use Keep a Changelog.
3. Comece a tagear releases com SemVer (`v1.0.0`, `v1.1.0`, etc.)
4. No CLAUDE.md raiz, aponte pra esses tres artefatos. O agente precisa saber onde encontrar.

Voce nao precisa retroativamente documentar tudo. Comece a partir de agora. A proxima decisao significativa ganha o primeiro ADR. O proximo deploy ganha uma tag.

---

*Voce leu as 4 camadas. O [README](../README.md) tem o diagrama completo de como elas se conectam.*
