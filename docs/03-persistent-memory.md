# Camada 3 — Memoria persistente

*O que aprender entre conversas (e o que NAO aprender).*

---

## O problema

Voce trabalha com um agente de IA por 2 horas. Ele aprende seu estilo, entende seu projeto, sabe o que voce ja tentou. A conversa termina.

No dia seguinte, voce abre uma nova conversa. Ele nao lembra de nada. Voce explica tudo de novo. E de novo. E de novo.

Agentes sem memoria sao como um colega de trabalho que acorda todo dia com amnesia.

## A analogia

Pensa num caderno de anotacoes. Depois de cada reuniao importante, voce anota: "o cliente prefere X", "a decisao foi Y porque Z", "nunca mais usar a abordagem W".

Memoria persistente e esse caderno — mas pro agente. Ele salva o que aprendeu, e le de volta no inicio de cada sessao.

Mas tem um detalhe que muda tudo: **anotar coisa errada e pior que nao anotar nada.** Uma memoria desatualizada pode levar o agente a repetir erros que voce ja corrigiu, ou a tomar decisoes baseadas em contexto que nao existe mais.

## Os 4 tipos de memoria

Depois de meses usando esse sistema, chegamos em 4 tipos com propositos claros:

### 1. User (quem e o usuario)
Papel, experiencia, preferencias, conhecimentos. Permite ao agente adaptar respostas pro nivel certo. Um engenheiro senior recebe respostas diferentes de um estudante.

**Exemplo:** *"Head de Produtos, proficiente em analise de dados, novo em frontend React. Adaptar explicacoes de frontend usando analogias de backend."*

### 2. Feedback (o que funcionou e o que nao)
Correcoes e confirmacoes. **O tipo mais importante.** Evita que o agente repita erros e garante que acertos sejam mantidos.

**Exemplo:** *"Nao posicionar tecnologias como substitutas. Prompt engineering + context engineering: ambos importam. Argumentar pelo complementar."*

A estrutura que funciona: regra + **Why** (por que o usuario disse isso) + **How to apply** (quando e onde aplicar). Sem o "why", voce segue a regra cegamente. Com ele, consegue julgar casos ambiguos.

### 3. Project (o que esta em curso)
Estado de iniciativas, decisoes de projeto, prazos, freezes. **Sao as memorias que mais envelhecem** — revise frequentemente.

**Exemplo:** *"Merge freeze a partir de 2026-04-15 para release mobile. Nao propor PRs nao-criticos apos essa data."*

**Regra crucial:** converta datas relativas pra absolutas. "Quinta" vira "2026-04-17". Senao a memoria fica ilegivel depois.

### 4. Reference (onde encontrar coisas)
Ponteiros pra informacao em sistemas externos: dashboards, boards de projeto, canais de comunicacao. Permite ao agente saber ONDE buscar sem ter que perguntar toda vez.

**Exemplo:** *"Bugs de pipeline sao trackados no Linear, projeto INGEST. Dashboard de latencia em grafana.internal/d/api-latency."*

## O que NAO salvar

Isso e tao importante quanto o que salvar:

- **Padroes de codigo, convencoes, arquitetura** — isso esta no codigo. Ler e mais confiavel que lembrar.
- **Git history, quem mudou o que** — `git log` e `git blame` sao autoritativos.
- **Solucoes de debug** — o fix esta no codigo, o contexto esta no commit message.
- **Qualquer coisa que ja esta em CLAUDE.md** — duplicar e o comeco da divergencia.
- **Detalhes efemeros da conversa atual** — memoria e pra FUTURAS conversas.

## Como funciona na pratica

```
.claude/projects/{{path-encoded}}/memory/
├── MEMORY.md              ← Indice (ponteiros, 1 linha por memoria)
├── user_profile.md        ← Tipo: user
├── feedback_testing.md    ← Tipo: feedback
├── project_release.md     ← Tipo: project
└── reference_dashboards.md ← Tipo: reference
```

O `MEMORY.md` e um indice — sempre carregado no inicio da sessao. Cada memoria individual so e lida quando relevante. Isso economiza contexto.

## O que diferencia isso dos frameworks existentes

Sistemas como Mem0, MIRIX e MemoryOS atacam o mesmo problema com abordagens sofisticadas (embeddings, grafos, hierarquias multi-nivel). Sao otimos — e se voce tem infra pra rodar, vale explorar.

O que encontramos de diferente na pratica:

1. **Tipologia semantica explicita** (user/feedback/project/reference) em vez de categorias automaticas. O agente sabe o PROPOSITO de cada memoria, nao so o conteudo.
2. **Regras de NAO-salvamento** tao claras quanto regras de salvamento. A maioria dos frameworks foca em "o que capturar". Nos focamos igualmente em "o que ignorar".
3. **Expiração por tipo.** Nem toda memoria envelhece igual. A regra de expiracao depende do tipo:

| Tipo | Expira? | Regra |
|------|---------|-------|
| **user** | Raramente | Atualiza quando o perfil muda. Nunca deleta — sobrescreve. |
| **feedback** | Quando superado | Se uma regra e substituida por outra, remova a antiga. Se ainda vale, fica. |
| **project** | Quando o projeto termina | Sao as que mais envelhecem. Revise a cada 2-4 semanas. |
| **reference** | Quando a fonte muda | URL mudou? Dashboard migrou? Atualize ou remova. |

Identidade (SOUL.md, STYLE.md) **nunca expira** — cresce por acumulacao. Memorias de projeto expiram rapido. Tratar tudo igual e receita para contexto poluido.

## Como implementar

1. Crie a pasta de memoria no padrao do seu editor (`.claude/projects/.../memory/` pra Claude Code)
2. Comece com 1 memoria tipo `user` (perfil basico) e 1 tipo `feedback` (primeira correcao que voce fizer)
3. Use os templates em `/templates/memory/` como ponto de partida
4. Revise periodicamente seguindo a tabela de expiracao acima

---

*Proxima camada: [04 — Historico de decisoes](04-decision-history.md)*
