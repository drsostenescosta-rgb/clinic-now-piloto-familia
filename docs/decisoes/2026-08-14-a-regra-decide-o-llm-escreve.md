# A regra decide a ação; o LLM só escreve o texto

- **Data:** 2026-08-14
- **Status:** ✅ DECIDIDA e implementada
- **Vale para:** ClinicNow, Fase 1 (operação assistida) e além

## O que estava em jogo

O motor da Emily (`~/Applications/medgrowth/emily-vendas/emily.mjs`, 809 linhas) manda a mensagem
para o Claude e usa a resposta do modelo tanto para **decidir o que fazer** (`acao`:
responder / consultar_especialista / escalar_humano / opt_out) quanto para **escrever o texto**.

Isso funciona bem para vender. Não funciona para as regras invioláveis da Andreia.

## O problema concreto

As regras dela não são preferências de redação:

- "confirmação só com *sim* ou *confirmo* explícito";
- "conflito de agenda bloqueia — não existe agendar mesmo assim";
- "qualquer intercorrência de pós-operatório escala na hora";
- "cancelamento com menos de 24h em sexta ou sábado fala com a Andreia antes".

Se essas decisões vivem dentro de um prompt, não existe como **provar** que o modelo nunca vai
decidir diferente. Dá para testar 30 casos e passar em 30. O caso 31 é a cliente da Andreia.

E tem um problema mais imediato: a `ANTHROPIC_API_KEY` está **401 (inválida) desde 07/08/2026**,
registrado no README do emily-vendas e confirmado hoje com `node emily.mjs --check`. Um sistema em
que a regra de segurança depende de uma chave expirada não é um sistema de segurança.

## A decisão

Separar em três camadas, com autoridade explícita:

| Camada | Arquivo | Decide o quê | Depende de rede? |
|---|---|---|---|
| **REGRA** | `emily-vendas/regras.mjs` | a **ação** | não |
| **VOZ** | `emily-vendas/voz.mjs` | as **palavras** (apelido do serviço, hora falada, temperatura) | não |
| **LLM** | `emily-vendas/api.mjs` → `polirTexto()` | **polimento** do texto já escrito | sim, e é opcional |

O LLM recebe a ação **já decidida** e é instruído a não mudá-la. Se ele cair, alucinar ou a chave
expirar, `polirTexto()` devolve o texto da regra intacto e a operação continua. Foi verificado hoje:
a ponte roda com a chave 401 e as propostas saem normalmente, marcadas `texto_polido_por_llm: false`.

## O que isso comprou

- **Testabilidade.** 91 testes automatizados cobrem as regras, a voz, o ledger e o Agendor. Os cinco
  cenários críticos da Andreia rodam com `npm run shadow:andreia` e passam 5/5.
- **Auditoria.** Cada proposta carrega o código da regra que a produziu (`R5.CANCELAMENTO_TARDIO_SEX_SAB`)
  e a lista de bloqueios que valeram. A Andreia vê na tela por que a Emily decidiu aquilo.
- **Operação sem chave.** O piloto não fica refém do faturamento da Anthropic.
- **Fail-closed de verdade.** O default do motor é ESCALAR. Mensagem que nenhuma regra cobre vai
  para humano; não existe caminho "responde qualquer coisa".

## O que isso custou

- A Emily responde **menos** coisas sozinha do que responderia com o LLM decidindo. Muita mensagem
  cai em "escalar". Na Fase 1 isso é aceitável — é exatamente o modo assistido — mas é a métrica a
  vigiar: se 90% escalar, a Emily não tirou trabalho das costas dela.
- Cada regra nova é código, não linha de prompt. Mais lento de mudar, e é o preço da garantia.

## Limite conhecido do validador de polimento

`validarPolimento()` (em `api.mjs`) é uma **denylist**: ele reprova a reescrita do LLM quando ela
introduz número, moeda, promessa de resultado ou vocabulário que a decisão bloqueou. Denylist
sempre tem furo — `"faço um precinho especial"` e `"pro seu caso a drenagem resolve certinho"`
passam hoje.

Isso está inerte enquanto a chave estiver 401, mas não deixa de ser dívida. **O caminho definitivo
é allowlist**: o polido só pode reordenar e reescrever o que já estava no rascunho, sem introduzir
afirmação nova nenhuma. Enquanto a allowlist não existir, a regra prática é a que já vale — quem
aprova lê o texto antes de colar.

Registrado a pedido da auditoria de 14/08/2026 (rodada 3).

## O que faria mudar de ideia

Ligar o LLM como decisor exigiria, juntas: chave válida e monitorada; a suíte de eval com
gate [SEG] = 100% rodando em CI; e um período de shadow em que a decisão do LLM é comparada contra
a decisão da regra sem nunca substituí-la. Enquanto as três não existirem, a regra decide.

## Onde ver funcionando

```bash
cd ~/Applications/medgrowth
npm run shadow:andreia      # os 5 cenários críticos, 100% sintéticos
npm test                    # 91 testes
npm run api                 # sobe a ponte; o painel do ClinicNow consome
```
