# Kit de operação diária — Andréia Carvalho Aesthetics

Como rodar um dia de piloto na Fase 1. Escrito para ser seguido sem ninguém explicar por cima.

**A regra que vale acima de todas:** nenhuma mensagem sai sozinha. O ClinicNow escreve, uma pessoa
lê, e uma pessoa envia. Aprovar no painel **não envia nada** — libera o texto para copiar e colar.

---

## 1. Quem faz o quê

| Papel | Quem | O que faz |
|---|---|---|
| **Aprovador** | Sostenes (semana 1) → Andreia (a partir da semana 2) | Lê a proposta, aprova/edita/escala, cola no WhatsApp |
| **Dona da decisão** | Andreia | Tudo que a Emily escalar: clínico, valor fora da tabela, encaixe, cancelamento tardio |
| **Suporte técnico** | Sostenes | Sobe a ponte, resolve erro, não responde questão clínica |

Na semana 1 quem aprova é Sostenes, para a Andreia não gastar tempo aprendendo ferramenta antes de
ver valor. A partir da semana 2 ela assume a aprovação e ele fica só no suporte.

## 2. Os três momentos do dia

O piloto não pede que ninguém fique olhando a tela. São três paradas.

| Horário | Duração | O que fazer |
|---|---|---|
| **09:00** (antes do 1º atendimento) | ~10 min | Abrir o painel, limpar a fila da noite, confirmar quem vem hoje |
| **13:00** (intervalo) | ~10 min | Limpar a fila da manhã |
| **19:00** (fim do dia) | ~15 min | Limpar a fila da tarde + rodar a confirmação de véspera de amanhã |

Fora desses horários, o único motivo para abrir o painel é um alerta de **PRIORIDADE ALTA**
(urgência ou intercorrência de pós-operatório) — esses não esperam a próxima parada.

## 3. Subir o sistema

```bash
cd ~/Applications/medgrowth && npm run api
```

Ele imprime o estado logo de cara: nome da clínica, preflight, modo e as pendências abertas.
Se disser `preflight: REPROVADO`, está **correto** — ver a seção 6.

Em outra janela:

```bash
cd ~/Applications/clinic-now-app && npm run dev
```

Abrir <http://localhost:5190> → aba **Aprovações**.

Para o painel falar com a ponte (e não com a fila de demonstração), rode o app com:

```bash
cd ~/Applications/clinic-now-app && VITE_APROVACOES_API=http://127.0.0.1:4791 npm run dev
```

## 4. Como decidir cada cartão

Cada cartão mostra, nesta ordem: quem escreveu, o que escreveu, **qual regra decidiu**, o que ficou
proibido nessa resposta, o que acontece na agenda, e o texto proposto.

| Você lê | Você faz |
|---|---|
| Etiqueta verde **Responder** | Leia o texto. Se estiver bom, **Aprovar** e cole no WhatsApp. |
| Etiqueta laranja **Reperguntar** | A cliente respondeu algo ambíguo ("tá bom", 👍). Aprove a repergunta — não conte como confirmado. |
| Etiqueta vermelha **Escalar para a Andreia** | Não responda por conta própria. Aprove o texto de transição, cole, e **chame a Andreia**. |
| Etiqueta marrom **Bloqueado por conflito** | O horário tem outra cliente. Nunca mova a outra. Quem decide encaixe é a Andreia. |

**Sempre escreva seu nome em "Quem está aprovando agora".** Sem nome, o painel recusa a decisão —
de propósito: decisão anônima não é aprovação, e o registro precisa dizer quem foi.

**Editou o texto?** Ótimo — o painel registra o antes e o depois. É assim que a Emily melhora: no
fim da semana dá para ver o que você sempre reescreve.

## 5. Quando a Emily escala

1. Aprove e cole a mensagem de transição (ela nunca promete prazo que não temos).
2. Avise a Andreia pelo WhatsApp pessoal dela.
3. **Não responda a questão você mesmo** — nem Sostenes, nem ninguém que não seja ela.
4. Enquanto a Andreia não responde, a conversa fica com o humano. A Emily não retoma sozinha.

Escalações de **PRIORIDADE ALTA** (urgência, intercorrência de pós-op) vêm com faixa de alerta no
cartão. Essas você liga, não manda mensagem.

## 6. Por que o preflight está reprovado (e por que isso é certo)

O preflight reprova com 11 problemas, todos vindos de **5 respostas que faltam da Andreia**.
Enquanto isso durar, o sistema opera em **modo sintético**: a Emily **não oferece horário** e o
sistema só aceita apelidos no formato `Cliente Demo NN`.

> **O que a proteção de dados realmente faz — e o que ela não faz.**
> O sistema **recusa** e-mail, telefone (com ou sem separador, e com prefixo internacional), SSN e
> data de nascimento, e recusa qualquer identificador que não seja um apelido sintético.
> Ele **não sabe** reconhecer que "Larissa Mendes de Souza" é o nome de uma pessoa — reconhecer
> nome exigiria uma lista de nomes e erraria mesmo assim.
> Portanto: **a regra que protege dado de cliente é você não digitar dado de cliente.** O software
> reduz o risco; ele não substitui essa regra.

| # | O que falta | Consequência hoje |
|---|---|---|
| 2.3 | Grade de horários semanais (as respostas têm AM/PM ambíguo: "sexta 8:00 às 8:00") | A Emily nunca diz um horário; coleta a preferência e devolve |
| 6.5 | Três mensagens escritas no tom dela | Os rascunhos saem em tom genérico e o painel avisa isso |
| 9.1/9.2 | 3 conversas que viraram agendamento + 3 que não viraram, anonimizadas | Sem base de avaliação do prompt |
| 7.3 | Por quantos dias guardar os registros | Gate B: bloqueia dado real |
| 4.6 | Regra do sinal e da lista de espera | A Emily não menciona nenhum dos dois |

Isso não é bug. É o sistema se recusando a inventar o que ela não respondeu.

Conferir a qualquer momento:

```bash
cd ~/Applications/medgrowth
node emily-vendas/preflight.mjs \
  --clinica  ~/Applications/clinic-now-piloto-familia/config/clinica-config.json \
  --agenda   ~/Applications/clinic-now-piloto-familia/config/agenda-config.json \
  --operacao ~/Applications/clinic-now-piloto-familia/config/operacao-assistida.json
```

## 7. Fim de semana: os números

```bash
cd ~/Applications/medgrowth
node emily-vendas/ledger.mjs stats        # o que aconteceu na semana
node emily-vendas/ledger.mjs verificar    # a cadeia de aprovações está íntegra?
```

`verificar` tem de dizer **Cadeia ÍNTEGRA**. Se disser **QUEBRADA**, o arquivo de aprovações foi
mexido — pare e investigue antes de continuar.

> **O que "ÍNTEGRA" prova, e o que não prova.**
> A verificação compara duas coisas: a cadeia de hash *dentro* do arquivo e uma âncora externa
> (`.aprovacoes-ancora.json`) com o total de eventos e o último hash. Com isso ela detecta:
> linha editada no meio, linha apagada, arquivo truncado e arquivo reescrito por inteiro.
> O que ela **não** faz: impedir quem tem acesso ao seu computador de reescrever os **dois**
> arquivos de forma coerente. Isto é detecção de acidente, bug e edição descuidada — **não é
> prova criptográfica contra alguém mal-intencionado com acesso à máquina.**
> Se um dia isso precisar valer como prova para terceiros, o caminho é guardar o último hash
> fora da máquina (e-mail diário para você mesmo já resolve).

### Base de comparação (medida ANTES do piloto, com a Andreia, em 11/08/2026)

| Métrica | Antes do piloto | Meta em 7 dias |
|---|---|---|
| Mensagens por dia | 15 (normal) a 30 (dia cheio) | — |
| Tempo até a primeira resposta | ~10 min **quando ela está livre** (sem medida quando está atendendo) | ≤ 15 min sempre |
| Faltas sem avisar | 4 nas últimas 4 semanas | menos que 4 |
| Trabalho administrativo | 1h30 a 2h por dia | reduzir pela metade |

O que ela disse que quer ver em 7 dias: **agenda confirmada sem mensagem repetida**, **pedido novo
respondido em até 15 minutos** e **um plano piloto de divulgação**.

### As métricas novas, que só existem por causa do ledger

| Métrica | Onde | O que significa |
|---|---|---|
| `taxa_aprovacao_sem_edicao` | `ledger.mjs stats` | Quanto do que a Emily escreve serve como está. Subindo = ela está aprendendo o tom |
| `por_decisao.escalada` | idem | Quanto ela ainda joga para a Andreia. **Se ficar alto, o piloto não tirou trabalho das costas dela** |
| `editadas` | idem | O que sempre é reescrito vira regra nova na semana seguinte |

## 8. Limites honestos desta fase

- Nenhuma mensagem é enviada pelo sistema. O envio é humano, sempre.
- A Emily não afirma disponibilidade de horário enquanto a grade estiver pendente.
- A integração com o Agendor está implementada mas **desligada**: falta o token da Andreia
  (ver `docs/decisoes/2026-08-14-fonte-de-verdade-agenda-agendor.md`).
- O polimento de texto por IA está indisponível: a `ANTHROPIC_API_KEY` está expirada (401). Isso
  **não para a operação** — as regras e os textos funcionam sem ela.
- Nada de dado real de paciente entra no sistema antes de: Auth + RLS por dona (E2) aplicados e
  política de retenção definida (pendência 7.3).
