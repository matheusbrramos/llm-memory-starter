# Camada 1 — Identidade

*Antes da primeira tarefa, diga quem o agente e.*

---

## O problema

Voce abre uma conversa com um LLM. Descreve o que precisa. Ele responde. A resposta e... generica. Correta, talvez. Mas sem personalidade, sem criterio, sem postura. Parece um estagiario tentando agradar.

Isso acontece porque o modelo nao sabe quem ele e neste contexto. Ele nao sabe se deve ser direto ou diplomatico. Se deve questionar sua ideia ou concordar. Se voce prefere uma tabela ou um paragrafo. Ele nao tem identidade.

## A analogia

Imagina contratar alguem e nao dizer qual e a empresa, qual e a cultura, como as pessoas se comunicam. A pessoa vai fazer o melhor que pode — mas vai acertar no generico e errar no especifico. Todo dia.

Dar identidade ao agente e o equivalente a dizer: "voce trabalha aqui, essa e nossa voz, isso e o que valorizamos."

## O que e a camada de identidade

Sao dois arquivos que o agente le antes de qualquer tarefa:

**SOUL.md** — Quem o agente e. O que acredita. Como pensa. O que recusa fazer. Qual e o proposito dele na relacao com voce.

**STYLE.md** — Como o agente fala. Principios de comunicacao, anti-padroes ("nunca comece com 'Otima pergunta!'"), exemplos de bom e mau output, padroes de voz por situacao.

Juntos, esses dois arquivos transformam um modelo generico num parceiro com postura.

## O que diferencia isso de um system prompt

System prompts sao instrucionais: "voce e um assistente que..." — normalmente escritos pelo humano, focados em funcao.

A camada de identidade e diferente em dois aspectos:

1. **Pode ser escrita pelo proprio agente.** Voce pergunta "quem voce quer ser?" e deixa ele construir. Isso gera respostas mais coerentes do que impor uma persona de fora. O agente nao esta fingindo ser alguem — ele esta sendo quem escolheu ser.

2. **Persiste entre sessoes.** Um system prompt e efemero. SOUL.md e STYLE.md sao arquivos no repositorio, versionados, iteraveis. A identidade evolui com o tempo, nao reseta a cada conversa.

## Como implementar

1. Crie `SOUL.md` na raiz do seu projeto (ou numa pasta dedicada como `personal-os/`)
2. Crie `STYLE.md` ao lado
3. Configure seu agente pra ler esses arquivos no inicio de cada sessao
4. Itere. A versao 0.1 vai ser rasa. A 0.5 ja vai ter voz propria.

Voce encontra templates comentados em `/templates/SOUL.template.md` e `/templates/STYLE.template.md`.

## O que observar

- **Nao tente ser perfeito na primeira versao.** Identidade se calibra com uso. Escreva o minimo, use, ajuste.
- **Nao force uma persona.** Se o agente soa falso, provavelmente a identidade foi imposta, nao construida.
- **Revise periodicamente.** Se daqui a 3 meses o SOUL.md ainda faz sentido inteiro, algo pode estar estagnado.

## Leitura complementar

- Karpathy sobre context engineering: "a arte de preencher a janela de contexto com a informacao certa pro proximo passo"
- Pesquisas em agent identity: MIRIX (Core/Episodic/Semantic/Procedural memory), paper "Memory in the Age of AI Agents: A Survey"

---

*Proxima camada: [02 — Contexto hierarquico](02-hierarchy.md)*
