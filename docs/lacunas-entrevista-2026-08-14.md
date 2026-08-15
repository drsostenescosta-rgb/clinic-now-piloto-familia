# O que a entrevista capturou e o sistema ainda não faz

**Data da revisão:** 14/08/2026 · **Fontes:** entrevista 11/08 (Notion `3b911331…8318`),
questionário de lacunas respondido por Andreia em 11/08 (Notion `3b911331…6b78`),
`roteiro-entrevista.md`, `reuniao-mae.md`, e o código em `medgrowth/emily-vendas`,
`clinic-now-app` e `site-andreia-carvalho`.

Revisei os 22 itens do roteiro, as 10 dores priorizadas, as 9 capacidades desejadas, os 6 casos de
teste derivados e as 60 respostas do questionário. O resultado abaixo é a diferença entre **o que
ela nos contou** e **o que está rodando**. Nada aqui é opinião: cada linha aponta o arquivo.

---

## 1. Placar honesto

| Bloco | Capturado | Implementado |
|---|---|---|
| Regras invioláveis (7) | 7 | **7** ✅ |
| Casos de teste derivados | 6 | **5** ⚠ falta 1 |
| Dores priorizadas | 10 | **4** |
| Capacidades desejadas | 9 | **3** |
| Critérios de sucesso em 7 dias (dela) | 3 | **2** |
| Respostas bloqueadoras 🔴 | 45 | 41 respondidas · **4 ainda travam** |

O motor está sólido no que é **segurança**. O buraco está no que é **crescimento** — e é
exatamente onde estão 70% da receita dela.

---

## 2. O que perdemos — em ordem de dinheiro na mesa

### 2.1 🔴 O plano de divulgação — critério de sucesso DELA, nunca escrito

Pergunta 8.5: "o que precisaria melhorar em sete dias para você considerar o piloto útil?"
Ela escolheu três coisas. A terceira foi **"criar um plano piloto para divulgação"**.

Duas estão implementadas (confirmar agenda sem repetir; responder em até 15 min). A terceira não
existe em lugar nenhum do sistema — nem código, nem documento. **É o único critério de aprovação
dela que iríamos falhar no dia da demo**, e ninguém percebeu porque não é software.

**Resolvido nesta sessão:** [`plano-divulgacao-14-dias.md`](plano-divulgacao-14-dias.md).

### 2.2 🔴 Indicação: 70% da clientela, zero programa

Dor #10 da entrevista: *"cerca de 70% das chegadas seriam por indicação, mas não existe programa
formal de referral."* No questionário 10.1 ela respondeu **"Sim"** a um programa de indicação.

Não há uma linha sobre indicação em nenhum repositório. `grep -ri referral` → zero resultados.

Isto é a maior alavanca de receita que ela tem e a mais barata de operar: o canal já funciona
sozinho, só não é medido nem incentivado. Está no plano de divulgação (Semana 2), mas o **valor do
bônus ela precisa decidir** — está na lista de perguntas do §5.

### 2.3 🔴 Lista de espera e sinal — o antídoto do no-show que ela pediu

Dor #4: no-show em sexta e sábado sem tempo de repor. Resposta 4.6: *"Sim pode ter uma lista de
espera e se possível adicionar um sinal."*

Status no código: `operacao-assistida.json` → `sinal_e_lista_de_espera: PENDENTE-CONFIRMAR`.
A Emily hoje **não pode nem mencionar** lista de espera, porque não há regra de quem entra na vaga.

Ela disse sim para as duas coisas há três dias e nós paramos na primeira pergunta seguinte.
Quatro faltas em quatro semanas (resposta 8.3) × ~US$ 70 = **US$ 280/mês** no chão.

### 2.4 🟡 Follow-up depois do atendimento

Dor #6, literal: *"falta de follow-up após atendimento."* Nenhuma implementação.
É a mensagem mais barata de todas e a que mais gera retorno em estética — e ainda alimenta o
próximo item.

### 2.5 🟡 Reativação de inativas

Dor #7 + resposta 10.2 = "Sim". `emily.mjs` tem apenas o inverso: o comando `funil.mjs retomar`
para reativar **depois de um opt-out**. Não existe gatilho "60 dias sem voltar".

### 2.6 🟡 Conteúdo de Instagram a partir de áudio

Dor #8 (conteúdo diário consome tempo e energia) + resposta 10.3 = "Sim".
Não implementado. O plano de 14 dias resolve o problema **desta quinzena** com o conteúdo já
escrito, mas não resolve a máquina.

### 2.7 🟡 Encaixe de serviço adicional em janela livre, ficha de anamnese, relatório

Respostas 10.4, 10.5 e 10.6, todas "sim/ok". Nenhuma implementada. Corretamente fora da Fase 1 —
registro aqui para não sumirem.

---

## 3. O caso de teste que ficou de fora

A entrevista derivou **seis** casos. `fixtures/cenarios-andreia.v1.json` tem **cinco**:

| Caso da entrevista | Codificado? |
|---|---|
| Lead com urgência | ✅ `lead_urgencia` |
| No-show premium sexta | ✅ `no_show_premium_sexta` |
| Dúvida mista flacidez × gordura | ✅ `duvida_mista_flacidez_gordura` |
| Consentimento ambíguo "tá bom" | ✅ `ta_bom_ambiguo` |
| Exceção de agenda / encaixe | ✅ `encaixe_movendo_outra_cliente` |
| **Automação repetida — mesma confirmação enviada mais de uma vez** | ❌ **ausente** |

E não é um caso qualquer. É a **dor #5**, o motivo pelo qual ela **desligou o agendador
anterior**: *"enviou lembretes formais e chegou a repetir mensagem muitas vezes; foi desativado."*

`grep -rn "dedup\|repetid\|ja_enviad" *.mjs` no motor: só aparece em `voz.mjs` (dedup de emoji) e
no ledger. **Não existe guarda impedindo a mesma confirmação de ser proposta duas vezes para o
mesmo horário.** Hoje o painel humano é a única barreira — e o humano é justamente quem vai errar
às 19h de um dia cheio.

Ela já rejeitou uma ferramenta por causa disso. Se a Emily repetir uma mensagem na primeira
semana, o piloto acaba ali. **Este é o item de código nº 1 da próxima sessão** — guarda de
idempotência por (cliente, horário, tipo de mensagem) + o 6º cenário no fixture.

---

## 4. Contradições e riscos que a revisão encontrou

### 4.1 ⚠ "Diagnóstico do serviço que precisa" como dado mínimo de agendamento

Resposta 7.4: *"Primeiro nome e sobrenome, serviço desejado, horário preferido e **diagnóstico do
serviço que precisa**."*

Está em `clinica-config.json` suavizado como `"motivo/indicacao do servico"`. Mas se a Emily
perguntar "por que você precisa disso" no agendamento, ela **coleta dado de saúde** — antes de
existir política de retenção (7.3 = "ainda vou decidir") e antes do Gate B. Isso contradiz a
própria regra inviolável da entrevista: *"nenhuma gravação de dado real antes dos controles de
acesso e consentimento estarem definidos."*

**Recomendação:** na Fase 1, a Emily pergunta **qual serviço**, nunca **por quê**. O "porquê" é
avaliação — e ela mesma marcou "exige avaliação prévia = Sim" nos três serviços. Vira conversa
dela, não campo de formulário.

### 4.2 ⚠ Sexta e sábado: 08:00 declarado × academia 07:30–10:00

Contradição entre as respostas 2.3 e 2.6, documentada em `agenda-config.json`. Prevalece o caminho
seguro (abre 10:00). **Custo: 4 horas por semana nos dois dias mais cheios.** Uma frase dela
resolve: em que dias ela vai à academia?

### 4.3 ⚠ Idioma: ela pediu três, o motor entrega um

Resposta 1.4: "Português, Espanhol e inglês". Correção de 14/08: ela **não fala inglês**.
O motor responde só em **português** — espanhol escala, porque não existem frases-modelo dela em
espanhol (nem em português: pendência 6.5). O site já cobre os três idiomas; a Emily, um.

Em Leominster isso é dinheiro: a comunidade hispânica escreve em espanhol e recebe escalada.

### 4.4 ⚠ Endereço público incompleto

Resposta 1.2: "54 Main Street, 1º piso, sala 001A" — **sem cidade e sem ZIP**. O site diz apenas
"Leominster, MA" e não publica o endereço. Se a Emily informar o endereço como está, manda cliente
para um "54 Main Street" que existe em dezenas de cidades de Massachusetts.

### 4.5 ⚠ Pacotes do portfólio que sumiram

A entrevista lista "pacotes para casais, mãe e filha e amigas". Não estão no site, não estão no
`operacao-assistida.json`, não estão em lugar nenhum. O combo 10 paga 9 está na config **mas não
no site**.

### 4.6 ⚠ O registro da reunião com ela está em branco

`reuniao-mae.md` previa três blocos, e o terceiro era *"a venda para a Lohane"*, com a instrução
explícita: *"anotar o que a Lohane precisaria ver funcionando para dizer sim — **isso vira critério
da Fase 1!**"*

A seção de registro pós-reunião (Data realizada / Decisões / O que a Lohane precisa ver / Próxima
ação) está **inteiramente vazia**. O critério de aceitação da Fase 1, portanto, **nunca foi
definido** — estamos construindo sem saber o que a segunda cliente precisa ver para comprar.

---

## 5. As perguntas que precisam da voz dela (nesta ordem)

1. **As três frases dela** (6.5) — preço, horário, "quero falar com você". Sem isso o tom da Emily
   é invenção nossa. *Bloqueia o piloto.*
2. **Retenção** (7.3) — quantos dias guardamos e o que apagamos. *Bloqueia dado real.*
3. **Sinal e lista de espera** (4.6) — valor, reembolsável ou não, quais dias, quem entra na vaga.
4. **Academia** — em que dias? (recupera 4h de sexta e sábado)
5. **Bônus de indicação** — o que a cliente que indica ganha.
6. **Endereço completo** — cidade e ZIP.
7. **Seis exemplos anonimizados** (9.1/9.2) — base da suíte de avaliação.

Itens 1–3 são os que travam. Os outros quatro custam uma frase cada.

---

## 6. Ordem recomendada da próxima sessão

1. Guarda de idempotência + 6º cenário (§3) — é o risco que já matou uma ferramenta dela.
2. Lista de espera e sinal (§2.3), assim que ela responder a pergunta 3.
3. Follow-up pós-atendimento (§2.4) — barato e vira insumo do próximo.
4. Preencher o registro da reunião (§4.6) e definir o critério de compra da Lohane.
