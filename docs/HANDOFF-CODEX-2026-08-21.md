# Prompt de continuidade para o Codex — WhatsApp da Andréia

Copiar o bloco abaixo inteiro como primeira mensagem numa sessão nova do Codex.

---

Você vai continuar um trabalho de integração que está a poucos passos de rodar. Leia tudo
antes de executar qualquer coisa. Responda em português.

## Contexto

Estou colocando a Emily (assistente de IA) para atender no WhatsApp da minha mãe, Andréia
Carvalho, que tem uma clínica de estética em Leominster, Massachusetts, e trabalha sozinha.
O site, o painel de aprovação e o motor de decisão já estão prontos e verificados. Falta
exclusivamente conectar o número dela à WhatsApp Business Platform pelo modo **Coexistence**
— que é o modo em que o app do celular continua funcionando normalmente.

## A regra que manda em tudo

O número **+1 (978) 600-3658** é a única ferramenta de trabalho dela. Perder o número, as
conversas ou o acesso pelo celular destrói o negócio dela.

Três proibições absolutas, sem exceção e sem "só para testar":

1. **Nunca confirme código SMS** para esse número em nenhum fluxo da Meta.
2. **Nunca execute o passo 2 do procedimento de migração do 360dialog** — ele manda entrar no
   WhatsApp comum com o número dela e reinstalar o Business. É tirar a ferramenta de trabalho
   do celular dela.
3. **Nunca desinstale o WhatsApp Business** — desinstalar desconecta a conta.

Além disso: **não apague nada na Meta sem autorização explícita do Sostenes na hora.** Existe
uma remoção que provavelmente resolve o problema e que está deliberadamente bloqueada — está
explicada abaixo.

## Leia primeiro, nesta ordem, e pare em 3 arquivos

1. `~/Applications/clinic-now-piloto-familia/docs/whatsapp-estado-2026-08-21.md` — o
   diagnóstico atual. **É o documento que manda.**
2. `~/Applications/clinic-now-piloto-familia/docs/whatsapp-estado-2026-08-17.md` — inventário
   de IDs. O diagnóstico dele está superado; o inventário vale. Contém uma nota de correção
   sobre uma citação inventada — leia essa nota.
3. `~/Documents/Repositorio Sostenes/00_CENTRAL/06_BRAIN/00-INDEX.md` — como o Sostenes
   trabalha e o que já foi decidido.

## Onde exatamente o processo parou

## Atualização posterior do suporte — 21/08, 03:09 UTC

Maria Marcelyna, líder regional de suporte da 360dialog, pediu capturas da entrada pendente
`Not verified` dentro da WABA. Ela declarou que a equipe não tem visibilidade suficiente para
confirmar os próximos passos sem ver essas telas. Portanto, este handoff **não autoriza uma
nova tentativa** até que as evidências solicitadas sejam entregues e o suporte esclareça o
estado técnico do cadastro.

Antes de qualquer nova tentativa, reunir:

1. captura da entrada `Not verified` dentro da WABA, com o contexto da conta visível;
2. captura atual de `Plataforma do WhatsApp Business` no celular;
3. captura dos parceiros do portfólio e da WABA;
4. inventário dos dispositivos vinculados e confirmação de backup recente.

“Enter a new phone number” sozinho **não identifica Coexistence**. Essa opção também pode levar
ao onboarding padrão com SMS/voz. Só continuar quando a interface disser explicitamente que
está conectando o WhatsApp Business App existente e o Hub permanecer marcado `Coexistence`.

No pop-up de Embedded Signup da Meta, aberto pelo 360dialog Hub
(`app.360dialog.com/client/93fqW7piCL/channel` → *Continue onboarding*), chega-se à tela:

> **"Adicione seu número de telefone do WhatsApp — Choose how you want to be identified when
> sending messages"**

com três opções:

1. `Enter a new phone number`
2. `Use a display name with a virtual number instead`
3. `Andreia Carvalho Estetica — +1 978-600-3658 · Andreia Carvalho Estetica`

Escolhemos a **3**. Ela é o registro velho dentro da WABA (phone number ID
`1287805514415120`, status *Não verificado*), sobra da primeira tentativa. Isso joga direto
em verificação por SMS e devolve o erro `2388091` no clique.

E o SMS **nunca vai funcionar** nesse número: a Meta desregistra o número do app para
verificar por SMS, e o número dela está ativo no WhatsApp Business. Por isso o Coexistence
existe — nele o código chega **dentro do WhatsApp**, não por SMS.

## O que fazer, nesta ordem

### Passo 1 — confirmar que a IA da Meta foi desligada

Descobrimos que o **assistente de IA da Meta** está respondendo clientes dela dentro do app,
sem ninguém ter configurado. Ela recebeu instruções para desligar em
`WhatsApp Business → Ferramentas → Meta AI / Agente de negócios`. Confirme com o Sostenes se
foi desligado e se ele tem o print.

Hipótese (não é fato): a IA da Meta pode ocupar o mesmo lugar que o Coexistence precisa.
Desligar é grátis e não apaga nada, então vem antes de tudo.

### Passo 2 — bloqueado: registrar o fluxo até a tela de seleção

Esta variante nunca foi testada e **não pode ser chamada de sem risco**. A próxima gravação
deve parar antes de selecionar ou submeter o número, salvo se o suporte responder às evidências
e o ramo de Coexistence estiver identificado explicitamente na própria interface.

Caminho completo, do zero:

1. `app.360dialog.com/client/93fqW7piCL/channel` → **Continue onboarding**
2. Plano: **Regular US$ 59/mês** (já aprovado pelo Sostenes; só cobra quando o número ativa)
3. Telefone: `9786003658` — **exatamente 10 dígitos**, com o 🇺🇸 +1 no seletor.
   *Numa tentativa anterior o campo ficou com `97860036582`, um dígito a mais.*
4. *"Is +1 978 600 3658 already on WhatsApp?"* → **`Yes, Business app`**
   ← este passo é obrigatório; sem ele o cabeçalho não mostra a etiqueta `Coexistence`
5. *"Do you have a Facebook account?"* → **`Yes, I have a Facebook account`**
   (a outra opção exige que o número não esteja em nenhum Business Manager, e o dela está)
6. **Confirme que o cabeçalho mostra a etiqueta `Coexistence`.** Se não mostrar, pare e volte.
7. **Embedded Signup** → abre pop-up da Meta. Se o Chrome bloquear, permita pop-ups para
   `app.360dialog.com`.
8. Na tela do número: **pare e registre a tela inteira**. `Enter a new phone number` só poderá
   ser usado se essa tela estiver inequivocamente dentro de `Connect your existing WhatsApp
   Business app`/Coexistence e houver autorização humana para continuar.
9. O esperado é que chegue **uma mensagem dentro do WhatsApp dela**, vinda da conta oficial do
   Facebook, com QR code ou código de acesso. Ela toca na mensagem e escaneia o QR mostrado na
   tela do computador — o botão *"Conectar-se à Plataforma do WhatsApp Business"* já está na
   tela dela.
10. Depois vem a confirmação de compartilhar até 6 meses de histórico.

**Grave a tela do começo ao fim.** O suporte do 360dialog pediu esse vídeo e é o que destrava
a resposta técnica deles.

**Se aparecer SMS ou voz:** pare antes de solicitar ou confirmar código, registre a tela e envie
ao suporte. Isso não prova sozinho a causa técnica, mas prova que o gate de Coexistence não está
claro o suficiente para este número crítico.

### Passo 3 — a remoção bloqueada

Se o passo 2 falhar, a causa provável é o cadastro pendente `1287805514415120` dentro da WABA
`922288694278090`. Removê-lo é o que provavelmente destrava.

**Não remova por conta própria.** Há relatos de cooldown da Meta de **1 a 2 meses** para
reusar um número que saiu de uma WABA. Os relatos descrevem números que chegaram a ficar
conectados; o dela nunca ficou — mas isso não está provado. Dois meses de trava no número de
trabalho dela é pior do que qualquer atraso.

Existe uma pergunta em aberto no suporte exatamente sobre isso (ver abaixo). Só remova com
resposta do suporte **ou** com autorização explícita do Sostenes na hora, dita naquele momento.

### Passo 4 — plano B, se o Coexistence não destravar

Sostenes já autorizou avaliar um **número novo em paralelo**, e a lógica é esta: número novo
**não é migração**. O celular dela continua igual, com os clientes de sempre. O número novo
recebe só o que **nasce** no sistema — botão do site, link do Instagram, QR na clínica.

A troca é de uma constante: `wa.me/19786003658` aparece em
`~/Applications/site-andreia-carvalho/dados.js` e é propagada para as três línguas no build.
Trocar, rodar o build e publicar leva minutos. **É reversível**: quando o Coexistence
destravar, aponta-se de volta.

Duas rotas para o número novo, nesta ordem de preferência:
- **`Use Meta-provided number`** / `Use a display name with a virtual number instead` — opções
  que aparecem no próprio fluxo do 360dialog. Sem chip, sem SIM. **Verifique as limitações
  antes de comprometer** — ninguém conferiu ainda.
- Número americano de VoIP (Telnyx, Twilio) que receba SMS e nunca tenha sido usado no WhatsApp.

## Estado do chamado no suporte

Thread do Gmail `1a0221ec56810ea8`, com `support@360dialog.com`, assunto começando com
*"Escalate to your HUMAN technical team, please"*. Conta 360dialog: Direct Client `93fqW7piCL`.

**Eles pediram:** um vídeo do fluxo inteiro.
**Nós perguntamos e ainda não responderam:** o cadastro pendente `1287805514415120` precisa
sair da WABA para o QR aparecer? Isso dispara cooldown?

Eles insistem que a "faixa azul" prova BSP anterior. **Está errado** — é o aviso da IA da Meta
("A IA está respondendo nesta conversa"). Já respondemos isso por escrito com a evidência.
Se voltarem ao mesmo argumento, aponte para a resposta de 03:1x na thread.

## Pendências menores, que não bloqueiam

- Horários de trabalho dela → liga `AGENDA.mostrarGrade` em `dados.js`
- Google Place ID → seção de avaliações do site
- 6 exemplos anonimizados + 3 frases aprovadas por ela → o preflight ainda falha nisso
- Trigger automático da purga de 90 dias no Supabase
- **Revisar o que a IA da Meta já respondeu para as clientes dela** — é conversa com paciente
  e ninguém olhou
- `Contact book for WhatsApp` está ligado no portfólio (a Meta guarda contatos das clientes) —
  entra na política de privacidade

## Como quero que você trabalhe

- **Não invente citação, número nem fonte.** Se não conferiu, diga que não conferiu. Houve uma
  citação inventada neste projeto e ela está registrada com correção visível — não repita.
- **Verifique antes de afirmar que funciona.** "Deve funcionar" não vale.
- **Não aceite premissa de suporte sem contraprova.** Foi assim que se perderam horas atrás de
  um BSP que não existia.
- **Pare e pergunte antes de qualquer coisa irreversível.** Apagar, publicar, gastar, ou tocar
  no celular dela.
- Registre o que aprender em `docs/` do repositório, não só na conversa.
