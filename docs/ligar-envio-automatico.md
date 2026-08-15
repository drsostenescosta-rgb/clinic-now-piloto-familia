# Ligar o envio automático — os seus 15 minutos

**15/08/2026.** Sostenes: *"quero em ação, foi isso que prometi; mensagem enviada automaticamente."*

O código de envio está pronto e testado. **O que falta é o canal** — hoje não existe para onde
enviar. Este documento é a sequência exata, e os passos 1 a 3 só você pode fazer (criar conta e
entrar com credencial não é coisa que eu faça por você).

---

## A parte que quase ninguém sabe: dá para provar HOJE

A verificação Meta Business leva de 1 a 3 semanas — e é por isso que quase todo mundo acha que
não dá para começar. Mas a Meta dá **um número de teste na hora**, assim que você cria o app.
Com ele você manda mensagem real, automática, para **até 5 números que você mesmo verifica**.

O celular da Andreia é um desses cinco.

Então a ordem é: **prova esta semana com o número de teste**, verificação da empresa rodando em
paralelo, e o número real dela entra quando a Meta liberar. Você não fica esperando três semanas
para mostrar que funciona.

---

## Passo 1 — criar o app (10 minutos, você)

1. Entre em **developers.facebook.com** com o seu Facebook.
2. **Meus Apps → Criar App → Outro → Empresa.**
3. No painel do app, **Adicionar produto → WhatsApp → Configurar.**
4. Na tela que abrir você já vê:
   - um **número de telefone de teste** (é da Meta, não é o dela)
   - o **Phone number ID** — anote
   - um **token temporário de 24 horas** — anote

## Passo 2 — autorizar o celular da Andreia (2 minutos, você)

Na mesma tela, em **"Para"**, clique em **Gerenciar lista de números** e adicione o WhatsApp dela.
Ela recebe um código e confirma. Sem isso a Meta recusa o envio — de propósito, é a proteção do
número de teste.

## Passo 3 — me passar as duas coisas

`WHATSAPP_PHONE_NUMBER_ID` e o token. Eu configuro na nuvem e a partir daí o envio é automático.

> O token de 24h serve para a prova. Para não expirar todo dia, depois se gera um **token
> permanente de usuário do sistema** — eu te mostro quando chegarmos lá; não vale gastar tempo
> nisso antes de a coisa funcionar uma vez.

## Passo 4 — a verificação da empresa (paralelo, é da Andreia)

**Configurações do Business → Verificação da empresa.** Ela envia documento do negócio. É o passo
lento (1 a 3 semanas) e é o que libera sair do número de teste. Começa agora e roda sozinho
enquanto o piloto anda.

## Passo 5 — o número dela, e a correção de uma informação que eu dei errada

**Em 14/08 eu recomendei número novo, dizendo que o número que entra na Cloud API sai do WhatsApp
normal. Isso era verdade e deixou de ser.** A Meta lançou o **Coexistence** (app do WhatsApp
Business + Cloud API no MESMO número): rollout em maio/2025, disponível em todos os países desde
maio/2026. Eu estava trabalhando com informação de antes disso.

Com Coexistence ligado:

- ela **continua usando o WhatsApp no celular dela**, normal;
- **o histórico de conversas fica**;
- mensagem nova sincroniza **nos dois lados em tempo real** — o sistema responde e ela vê no
  aparelho dela, como se ela tivesse respondido;
- ela pode assumir qualquer conversa a qualquer momento, digitando no celular.

**Condição:** precisa ser o **WhatsApp Business** (o app grátis, verde), não o WhatsApp pessoal.
Se ela usa o comum, a migração para o Business mantém as conversas e ela mesma faz em minutos.

**Recomendação corrigida: manter o número dela.** Número novo era a resposta certa para o mundo
sem Coexistence. Hoje, número novo só significa jogar fora o número que as clientes conhecem.

Isso deixa de ser bloqueio. Os passos 1 a 4 continuam valendo.

---

## O que a Emily vai enviar sozinha — e o que não vai

O que muda é quem aperta o botão. Quem decide continua sendo a regra.

| A regra classificou como… | O que acontece |
|---|---|
| **responder** (preço de serviço nomeado, descoberta, horário, "não temos lista de espera") | **A Emily manda sozinha** |
| **reperguntar_confirmacao** ("tá bom" não confirma) | **A Emily manda sozinha** |
| **escalar** (dúvida clínica, urgência, intercorrência de pós-op, pergunta pessoal, valor fora da tabela, cancelamento tardio de sexta/sábado, idioma que ela não fala) | Vai para a Andreia. Nada sai |
| **bloquear** (conflito de agenda) | Vai para a Andreia. Nada sai |

E mais três travas, que valem mesmo para o que é auto-enviável:

- **Decisão com alerta não sai.** Alerta existe para alguém olhar.
- **Preflight reprovado não sai.** Hoje ele reprova por um item: seus seis exemplos.
- **Tom não validado pela Andreia não sai.** Hoje `aprovado_pela_andreia` é `false` — o texto tem
  a voz dela como referência, mas ela ainda não leu.

**Ou seja: com a configuração de hoje, mesmo com o canal ligado, nada sairia sozinho.** Duas coisas
destravam, e as duas são dela: ler as três frases e mandar os seis exemplos. Dez minutos.

Se quiser ligar antes disso, é uma linha de configuração — mas aí a Emily manda para cliente um
texto que ninguém da casa leu, e o custo disso cai no WhatsApp da sua mãe.

## As três travas contra repetição

É a dor #5 da entrevista, e ela muda de tamanho quando o humano sai do meio. O agendador anterior
foi desligado por repetir mensagem. Com envio automático, repetir não é chateação: é um loop
mandando dez mensagens para a mesma cliente à meia-noite.

1. **Dedup por `wamid`.** A Meta **reenvia** o webhook quando não recebe resposta rápida. Sem
   isso, a primeira lentidão do banco vira mensagem duplicada.
2. **Dedup por conteúdo.** Texto igual para o mesmo contato não sai duas vezes em 3 horas.
   Compara normalizado — espaço e maiúscula não driblam.
3. **Teto de 4 mensagens por hora por contato.** Existe para bug virar silêncio, não enxurrada.

Tudo com teste automatizado (`test/envio.test.mjs`, 15 casos).

## Instagram — e por que ele é o mais fácil dos dois

A resposta 1.6 do questionário deixou o Instagram para a "segunda etapa". Pela dificuldade
percebida, isso está invertido: **o Instagram é mais fácil e ela não perde absolutamente nada.**

| | WhatsApp | Instagram |
|---|---|---|
| Ela continua usando o app dela? | Sim, com Coexistence ligado | **Sim, sempre.** Nunca houve esse problema |
| Perde histórico? | Não | Não |
| Verificação de empresa (1–3 semanas)? | Sim, para o número dedicado | **Não** |
| O que precisa | App Meta + Coexistence | Conta **profissional** + Página do Facebook vinculada |

**As regras reais do Instagram**, que definem o que dá para prometer:

- Só é possível responder a **quem interagiu nas últimas 24 horas** — DM, comentário ou resposta
  de Story. Isso encaixa exatamente no caso dela (responder quem chega) e proíbe DM frio, que a
  gente não faria de qualquer forma.
- Teto de **200 DMs automáticos por hora**. O volume dela é de 15 a 30 mensagens **por dia**.
- Bot de navegador, senha compartilhada e ferramenta não oficial **derrubam a conta**. Mesma
  regra do WhatsApp, mesmo veto: a conta dela é o ativo comercial, e o caminho é o oficial.

**O que checar no Instagram dela:** se a conta é **profissional** (Business ou Creator) e se tem
uma **Página do Facebook vinculada**. Sem os dois, a API não enxerga a conta. É configuração de
minutos, feita por ela no próprio app.

## O botão de pânico

`CLINICNOW_ENVIO_AUTOMATICO=false` na Vercel e nada mais sai — sem deploy, sem mexer em código.
Vale saber onde fica antes de precisar.
