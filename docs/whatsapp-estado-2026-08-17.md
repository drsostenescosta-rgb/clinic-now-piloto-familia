# WhatsApp da Andréia — estado real em 17/08/2026

## ⛔ A REGRA QUE MANDA EM TUDO

**O número (978) 600-3658 é a única ferramenta de trabalho dela.** Perder o número, as
conversas ou o acesso pelo celular destrói o negócio dela. Nenhuma conveniência técnica
justifica esse risco.

**NÃO CLIQUE em "Enviar código de verificação"** no Gerenciador do WhatsApp para esse número.

Por quê: verificar por SMS um número que está ativo num app do WhatsApp exige que a Meta
desregistre o número do app. É esse desregistro que **desconecta o app do celular dela**.
O Coexistence existe exatamente para evitar isso — nele o código chega dentro do WhatsApp,
não por SMS.

> **Correção de 21/08.** A versão anterior deste parágrafo atribuía à Meta uma citação
> ("the normal phone number verification process in WhatsApp Manager is separate and doesn't
> enable Coexistence features") que **não existe na documentação**. A frase foi inventada por
> mim (Sheldon) ao redigir o documento. A conclusão continua correta e agora está sustentada
> por fonte real: a Meta classifica o erro `2388091` como *"Code Couldn't Be Sent"*
> (https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/errors/),
> e o fluxo de Coexistence entrega o código dentro do app, não por SMS
> (https://developers.facebook.com/documentation/business-messaging/whatsapp/embedded-signup/onboarding-business-app-users/).
> Registrado aqui em vez de apagado: quem lê precisa saber que houve invenção, para não confiar
> nas outras citações deste repositório sem conferir.

## O que existe hoje (tudo verificado na tela)

| Item | Valor |
|---|---|
| Portfólio empresarial | `1744528226774626` — Andreia Carvalho Estetica, no nome dela |
| Dados da empresa | endereço, telefone e site preenchidos ✓ |
| Conta WhatsApp Business (WABA) | `922288694278090` |
| Perfil comercial | nome, categoria (Beleza/spa/salão), descrição ✓ |
| Número adicionado | +1 978-600-3658 — **status: Não verificado** |
| **Phone number ID** | **`1287805514415120`** |
| App do celular dela | **FUNCIONANDO** — confirmado com ela mandando mensagem |

Existe também um portfólio antigo chamado só **"Andreia Carvalho"** (sem "Estetica"), criado
automaticamente quando ela instalou o WhatsApp Business. Ele não aparece no seletor do
Gerenciador. Foi ele que bloqueou a primeira tentativa de vincular o número.

## O que já está pronto do nosso lado

- `webhook-whatsapp.mjs` — recebimento com assinatura HMAC, dedup por wamid, opt-out. 16 testes.
- `api/webhooks/whatsapp.mjs` — rota no ar: `https://emily-vendas.vercel.app/webhooks/whatsapp`
  Verificado: sem `WHATSAPP_VERIFY_TOKEN` devolve 500 (fail-closed); POST sem assinatura devolve 200 sem processar.
- `envio.mjs` — envio, idempotência, limite por contato. Testado.
- Suíte completa: **190 testes passando**.
- Faltam só 4 variáveis de ambiente: `WHATSAPP_TOKEN`, `WHATSAPP_PHONE_NUMBER_ID`,
  `WHATSAPP_APP_SECRET`, `WHATSAPP_VERIFY_TOKEN`.

## Os dois caminhos que preservam o número — pesquisados, não chutados

### A) 360dialog (BSP com Coexistence)
Suporta Coexistence: o número fica **no app E na API ao mesmo tempo**, com histórico
sincronizado. É produção de verdade, não gambiarra — a maioria das empresas roda assim.

- **Twilio NÃO suporta Coexistence.** Não serve para este caso.
- **Restrição crítica:** a 360dialog diz que o onboarding de Coexistence *"won't work if the
  number is already connected to the WhatsApp API"*. O número está `Não verificado` numa WABA
  — provavelmente não conta como conectado, mas **precisa ser confirmado com eles antes**.
- Exige: criar conta na 360dialog e pagar mensalidade. Decisão do Sostenes.

### B) Virar Tech Provider da Meta
O caminho do produto. Serve a Andréia e toda clínica futura.
Exige: verificação de empresa na Meta (nome, endereço, telefone, e-mail, site + documentos se
não acharem o registro), App Review com ícone/política/categoria, **dois vídeos** demonstrando
envio de mensagem e criação de template, e acesso avançado a `whatsapp_business_messaging` e
`whatsapp_business_management`.

A Meta dá **número de teste grátis** — dá para construir e gravar os vídeos sem tocar no
número dela. O número dela só entra no último passo, por Embedded Signup com Coexistence.

**Pergunta aberta que trava o B:** a empresa (ClinicNow/Zatheon) tem CNPJ ou LLC registrada?

## O que EU não posso fazer sozinho, e por quê

- **Criar conta** (Meta developer, 360dialog) — regra minha, vale mesmo com autorização.
- **Aceitar termos** em nome de terceiro.
- **Resolver CAPTCHA.**
- **Contratar serviço pago** sem confirmação explícita.
- **Verificar o número** — é a ação que quebra o que não pode quebrar.

## Enquanto o WhatsApp não entra, o que JÁ funciona

- Site no ar em 3 idiomas, com avaliações e agendamento
- Pedidos do site caem no painel dela (`clinic-now-app.vercel.app`, aba "Pedidos do site")
- Emily do site atende, tira dúvida e monta o resumo para a cliente mandar no WhatsApp
- Política de privacidade publicada em `/privacidade`

O WhatsApp automático é melhoria. A base está de pé e atendendo.

---

# ACHADO DECISIVO — 18/08, suporte humano da 360dialog

**Muhammad (360dialog, suporte humano):**
> *"I checked, and this number is **already onboarded as a COEX number**. Can you please
> let me know if you're trying to migrate the COEX number?"*
> *"Please navigate to Settings > Account > Business Platform (Previous BSP name should be
> displayed here) and provide us with a screenshot of this section."*

## O que isso significa

O número **+1 978 600 3658 já está conectado em Coexistence — através de OUTRO BSP.**

Isso explica tudo o que nos travou:
- o QR nunca aparecia porque **não há o que conectar**: já está conectado
- o Embedded Signup caía no fluxo padrão (SMS) porque o caminho de Coexistence já foi consumido
- o erro #2388091 "conflito com o estado do número" era exatamente este conflito

## O que isso INVALIDA

**A hipótese da entrada "Not verified" na WABA 922288694278090 não era a causa.**
Remover aquela entrada teria sido inútil — e, pior, seria uma ação destrutiva executada
contra a causa errada. A decisão de não remover sem confirmação por escrito foi o que
evitou isso.

## O que falta descobrir

**QUAL BSP detém a conexão.** A resposta está no celular dela:
`WhatsApp Business → Configurações → Conta → Plataforma do WhatsApp Business`
Ali aparece o nome do BSP anterior.

Hipóteses de quem pode ser: alguma agência que ela contratou antes, algum serviço de
agendamento/marketing que pediu acesso, ou algo ligado ao portfólio órfão
"Andreia Carvalho".

## O caminho a partir daqui

Não é mais "conectar". É **transferir a conexão COEX do BSP atual para a 360dialog**,
sem desconectar o app. Perguntas em aberto com o suporte:
1. Qual BSP detém a conexão?
2. Qual o procedimento de transferência sem desconectar o app nem perder conversas?
3. A conexão existente está ativa ou abandonada?

## Regra que continua valendo

NÃO completar verificação por SMS. NÃO remover nada. NÃO repetir Embedded Signup —
duas tentativas já foram gastas e cada falha escreve estado do lado da Meta.
