# Como conectar no WhatsApp dela — os três caminhos, sem enfeite

Pergunta do Sostenes em 14/08/2026. Resposta direta primeiro, depois o porquê.

**Hoje, Fase 1: o conector é você.** A mensagem chega no WhatsApp dela, você cola no painel, a
Emily propõe, você aprova e cola de volta. Zero integração, zero risco, funciona hoje à noite.
Não é gambiarra — é a decisão da mentoria de 07/08 (validar o roteiro antes de automatizar).

Os caminhos para tirar você do meio são três, e só um serve.

---

## 1. WhatsApp Cloud API — o único caminho oficial (o certo)

É a API da própria Meta. É o que `webhook.mjs` já implementa, e está **desligado** esperando
exatamente isto.

**O que precisa acontecer, em ordem:**

| # | Passo | Quem faz | Prazo real |
|---|---|---|---|
| 1 | Conta **Meta Business** verificada (documento da empresa) | Andreia (é o negócio dela) | 1–3 semanas |
| 2 | App no Meta for Developers + produto WhatsApp | Sostenes | 1 dia |
| 3 | Número dedicado registrado na Cloud API | Andreia decide o número | 1 dia |
| 4 | Templates de mensagem aprovados pela Meta | Sostenes escreve, Meta aprova | 2–5 dias |
| 5 | Webhook público com HTTPS + verificação | Sostenes | 1 dia |

**A pegadinha que quase todo mundo descobre tarde:** o número que entra na Cloud API **sai do
WhatsApp normal**. Ela não consegue mais usar aquele número no celular dela como sempre usou.
Duas saídas: um número novo só para a clínica (recomendo), ou migrar o atual e ela passar a
atender por uma caixa de entrada diferente.

**Custo:** a Meta cobra por conversa iniciada pelo negócio; conversa iniciada pela cliente tem
janela gratuita de 24h. No volume dela (15–30 mensagens/dia, quase toda iniciada pela cliente),
fica perto de zero.

**Pré-condições que o nosso código já exige antes de subir** (estão no `webhook.mjs`, não são
recomendação — ele recusa iniciar sem elas):
- `WHATSAPP_APP_SECRET` — todo POST validado por assinatura HMAC. POST forjado → 401.
- `ALERTA_WEBHOOK_URL` — urgência notifica a Andreia na hora, não espera painel.
- Suíte de eval com gate [SEG] = 100%.
- `clinica-config.json` sem nenhum `[PREENCHER`.

E mesmo depois de ligado: **na Fase 1 ele não responde sozinho.** O webhook chama o mesmo
`processarMensagem()` e a proposta cai no Painel de Aprovação. A automação de envio é uma decisão
separada, tomada depois de a Andreia ver o sistema funcionando.

## 2. WhatsApp Business App (o que ela provavelmente já usa) — não conecta

O aplicativo do WhatsApp Business **não tem API**. Dá para configurar saudação automática,
mensagem de ausência e catálogo — e é só isso. Não existe forma legítima de um programa ler as
mensagens dele.

Serve para uma coisa concreta hoje: a **mensagem de ausência** dela pode dizer que a resposta
chega em até 15 minutos, que é a meta do piloto.

## 3. Baileys, Evolution API e afins — **VETADO**

São bibliotecas que se passam pelo WhatsApp Web. Funcionam, são fáceis, e são a razão de números
serem banidos sem aviso e sem recurso.

O número da clínica da sua mãe é o ativo comercial dela. Perder o número é perder a agenda inteira
e o histórico com cada cliente. Isso está vetado no PRD §7 e continua vetado — **não é discussão
técnica, é gestão de risco do negócio da sua mãe.**

---

## O que eu faria, na ordem

1. **Esta semana:** operar do jeito Fase 1 (você cola). É o que prova o valor e gera os exemplos
   reais que ainda faltam (pendência 9.1/9.2).
2. **Em paralelo, começando já:** abrir a verificação Meta Business. É o passo mais lento (1–3
   semanas) e não depende de mais nada — enquanto ela roda, o piloto continua.
3. **Só depois de a Andreia dizer "isso me ajudou":** decidir número dedicado, ligar o webhook, e
   manter a aprovação humana por pelo menos duas semanas com o canal ligado.
4. **Automação de envio:** última coisa, e só com o gate de eval em 100% e ela pedindo.

**Antes do passo 2 preciso de uma decisão sua:** número novo para a clínica, ou migrar o atual?
Recomendo número novo — ela não perde o WhatsApp pessoal/atual e a migração deixa de ser um evento
de risco no meio do piloto.
