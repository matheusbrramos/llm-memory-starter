# Camada 2 — Contexto hierarquico

*Cada pasta conta sua propria historia.*

---

## O problema

Voce tem um projeto com 15 pastas. Site, docs, backend, marketing, infra. O agente abre o projeto e... ve tudo. Ou nao ve nada. Depende do que voce cola no prompt.

Se voce cola tudo: o contexto fica poluido, redundante, e o modelo se confunde com informacao conflitante.

Se voce nao cola nada: o modelo nao sabe onde esta, o que importa, o que ja foi decidido.

## A analogia

Pensa num escritorio fisico. Cada sala tem uma placa na porta: "Financeiro", "Marketing", "Engenharia". Quando voce entra numa sala, sabe o que se faz ali. Nao precisa ler o organograma inteiro da empresa pra entender o contexto local.

Contexto hierarquico e a mesma coisa: cada pasta tem o seu proprio arquivo de contexto, que explica O QUE esta ali e COMO trabalhar naquele espaco.

## Como funciona

O arquivo se chama `CLAUDE.md` (ou `.cursorrules`, ou qualquer padrao que seu editor de codigo use). A ideia e a mesma independente do nome:

```
Projeto/
├── CLAUDE.md              ← Visao geral, mapa, regras globais
├── site/
│   └── CLAUDE.md          ← Design system, deploy, o que NAO mexer
├── docs/
│   └── CLAUDE.md          ← O que tem em cada subpasta
├── marketing/
│   └── CLAUDE.md          ← Convencoes de naming, stack, fluxos
└── backend/
    └── CLAUDE.md          ← Arquitetura, endpoints, migrations
```

**A regra de ouro: informacao nunca se duplica.**

O CLAUDE.md da raiz e um **mapa** — mostra onde as coisas estao. Os CLAUDE.md das subpastas sao **guias locais** — explicam como trabalhar ali. Se uma informacao aparece em dois lugares, cedo ou tarde ela vai divergir. E contexto conflitante e pior que contexto nenhum.

## O que colocar em cada nivel

### CLAUDE.md raiz (o router)
- O que e este projeto (2 frases)
- Arvore visual das pastas com descricao inline
- Estado atual (versoes, status)
- Change tracking (onde estao CHANGELOG, ADRs, tags)
- Comportamento esperado do agente (inicio e fim de sessao)

### CLAUDE.md de subpasta (o guia local)
- O que esta nesta pasta e por que existe
- Convencoes especificas (naming, formatos, workflows)
- Relacao com outras pastas ("isso vive aqui, aquilo vive la")
- Stack/ferramentas relevantes a ESTA area
- Fluxo de trabalho local

## O que nao colocar

- **Dados sensiveis.** Credenciais, tokens, chaves — nunca num arquivo de contexto.
- **Conteudo que muda todo dia.** State files existem pra isso (ver camada 3).
- **Instrucoes genericas.** "Escreva codigo limpo" nao e contexto — e ruido.

## O papel da hierarquia no agente

Quando o agente trabalha numa subpasta, ele le:
1. O CLAUDE.md da raiz (contexto geral)
2. O CLAUDE.md da subpasta (contexto local)

Isso da a ele as duas coisas que precisa: **visao do todo** e **detalhe do local**. Sem redundancia.

A maioria dos editores de codigo com suporte a IA (Claude Code, Cursor, etc.) ja faz essa leitura automaticamente. Voce so precisa criar os arquivos.

## Como implementar

1. Comece pelo CLAUDE.md da raiz. Use o template em `/templates/CLAUDE.root.template.md`.
2. Adicione CLAUDE.md nas 2-3 subpastas onde o agente mais trabalha.
3. Nao crie pra todas as pastas de uma vez. So onde faz diferenca.
4. Mantenha atualizado — revise a cada mudanca estrutural significativa.

## Uma decisao de design que vale destacar

A maioria dos guias sobre CLAUDE.md foca em **localizacao** (onde o arquivo fica). O que funciona melhor na pratica e focar em **proposito**: cada CLAUDE.md tem um PAPEL no sistema. O da raiz e router. O da subpasta e guia. O do docs e indice. Quando voce pensa em papel e nao em localizacao, a hierarquia emerge naturalmente.

---

*Proxima camada: [03 — Memoria persistente](03-persistent-memory.md)*
