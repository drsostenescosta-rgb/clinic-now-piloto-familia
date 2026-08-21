# WhatsApp da Andréia — estado real em 21/08/2026 (madrugada)

Sucessor de `whatsapp-estado-2026-08-17.md`. Aquele documento continua válido para o
inventário de IDs; **este aqui manda no diagnóstico**, porque o diagnóstico de 17/08
estava errado.

---

## ⛔ A REGRA QUE MANDA EM TUDO (inalterada)

**O número +1 (978) 600-3658 é a única ferramenta de trabalho da Andréia.** Perder o número,
as conversas ou o acesso pelo celular destrói o negócio dela. Nenhuma conveniência técnica
justifica esse risco. Palavras de Sostenes, 17/08: *"se ela perder isso, tá destruído tudo"*.

Disso decorrem três proibições absolutas, que valem para qualquer agente que continue este
trabalho:

1. **Nunca confirmar código SMS** para esse número em nenhum fluxo da Meta.
2. **Nunca executar o passo 2 do procedimento de migração do 360dialog** — ele manda entrar
   no WhatsApp comum com o número dela e reinstalar o Business. Isso é literalmente tirar a
   ferramenta de trabalho do celular dela.
3. **Nunca desinstalar o WhatsApp Business** — a documentação diz que desinstalar desconecta
   a conta de forma irreversível.

---

## O que mudou hoje: três descobertas que derrubaram o diagnóstico anterior

### 1. Não existe BSP anterior. O número está livre.

O suporte do 360dialog (Muhammad, depois Maria) afirmou por duas vezes que o número
*"is already onboarded as a COEX number"*, com base numa **faixa azul** que eles viram ao
mandar mensagem para o número pelo WhatsApp.

Isso é falso, e a prova veio do celular dela:

- `WhatsApp Business → Configurações → Conta → Plataforma do WhatsApp Business` mostra uma
  tela de **CONECTAR**: título *"Conectar-se à Plataforma do WhatsApp Business"*, corpo
  *"Escaneie o QR code para se conectar"*, e um botão de conectar.
- **Não há nome de provedor. Não há opção de desconectar.**

Verificado também, do lado da Meta, que nada está ligado (tudo lido em tela, 20–21/08):

| Onde olhei | Resultado |
|---|---|
| Portfólio `1744528226774626` → Parceiros | "No Partners added yet" |
| WABA `922288694278090` → aba Parceiros | "0 partners are assigned" |
| Gerenciador do WhatsApp → número → menu "Mais" | só "Configurações de ligação" e "Registros de ligações" |

**Conclusão:** as horas gastas caçando "qual BSP tem o número" foram gastas atrás de algo que
não existe. A premissa veio do suporte e foi aceita sem contraprova por tempo demais.

### 2. A faixa azul é a IA da Meta, não conexão de API.

A Andréia mandou o print de uma conversa real com uma cliente (Ronise), datada de hoje:

- Aviso do sistema no chat: *"Sua empresa agora usa um serviço seguro da Meta para gerenciar
  esta conversa. Toque para saber mais."*
- No rodapé do mesmo chat: **"A IA está respondendo nesta conversa"** + botão
  **"Responder manualmente"**.

Isso é o **assistente de IA da Meta** respondendo dentro do app dela. A Meta sai da
criptografia ponta a ponta nas conversas que a IA lê — daí o aviso de "serviço seguro". É
esse aviso que o suporte vê como "faixa azul". Uma conexão de BSP não anuncia que uma IA está
respondendo.

**Isto é um achado de risco, não só de diagnóstico.** Uma IA sem nenhuma regra está
respondendo pacientes de estética em nome dela: sem as travas que escrevemos para a Emily
(nada de promessa de resultado, nada de resposta clínica, nada de preço inventado, nada de
alegação de calorias). Ninguém configurou isso deliberadamente — apareceu. **Pendência aberta:
revisar o que essa IA já respondeu.**

### 3. O SMS nunca poderia ter funcionado. O caminho era impossível por construção.

A Meta documenta o erro `2388091` como **"Code Couldn't Be Sent"** — falha em *entregar* o
OTP, não instabilidade de servidor (apesar do texto genérico "our servers are temporarily
unavailable, please wait 1 hour").

Razão estrutural: **não se verifica por SMS um número que está ativo num app do WhatsApp** —
a verificação exige desregistrar o número do app. É precisamente por isso que o Coexistence
existe, e no Coexistence **o código chega dentro do WhatsApp**, como mensagem da conta oficial
do Facebook, e não por SMS.

Ou seja: toda vez que apareceu tela de SMS, não era um passo do fluxo. Era o sinal de que
estávamos no fluxo errado.

---

## O que ainda bloqueia, e onde exatamente

Duas coisas foram corrigidas hoje no fluxo do 360dialog:

- **`Yes, Business app`** — a bifurcação *"Is +1 978 600 3658 already on WhatsApp?"*, com quatro
  opções (`No, It's not connected` / `Yes, Personal app` / `Yes, Business app` /
  `Yes, Business API`). Nunca havíamos marcado. Depois de marcar, o cabeçalho do Hub passou a
  exibir a etiqueta **`Coexistence`** pela primeira vez — prova de que o ramo certo foi ativado.
- **O número tinha um dígito a mais.** O campo estava com `97860036582` (11 dígitos). Corrigido
  para `9786003658`.

Mesmo assim o pop-up da Meta continua caindo em SMS. O ponto exato é a tela:

> **"Adicione seu número de telefone do WhatsApp — Choose how you want to be identified when
> sending messages"**

com três opções no seletor:

1. `Enter a new phone number`
2. `Use a display name with a virtual number instead`
3. `Andreia Carvalho Estetica — +1 978-600-3658 · Andreia Carvalho Estetica`

**A opção 3 foi a escolhida, e é o registro velho dentro da WABA** (phone number ID
`1287805514415120`, status *Não verificado*, resto da primeira tentativa). Escolher um número
já cadastrado ali não é "onboarding de um número do app" — é "termine de verificar este
cadastro pendente", e cadastro pendente só tem SMS. O erro `2388091` volta **no clique**, sem
que nenhum código chegue a ser pedido.

Session ID da última tentativa: `01a02234-cf4a-7cca-9347-4debb657ef82`.

### As duas saídas, e por que a ordem importa

| | Ação | Risco | Estado |
|---|---|---|---|
| **A** | Escolher **`Enter a new phone number`** e digitar o número, em vez do registro listado | **Zero.** Não apaga nada. É o que a doc do 360dialog manda ("Enter the phone number again and verify") | **Nunca testada** |
| **B** | Remover o cadastro pendente `1287805514415120` da WABA | **Desconhecido.** Há relatos de cooldown de 1–2 meses da Meta para reusar número que saiu de uma WABA. Os relatos descrevem números que chegaram a ficar conectados; o dela nunca ficou — mas não temos prova de que a distinção vale | **Bloqueada até o suporte confirmar** |

Sostenes recusou explicitamente autorizar B enquanto o cooldown não estiver esclarecido, e
essa recusa está correta: dois meses de trava no número dela é pior do que qualquer atraso.

### Hipótese nova, ainda não testada

A IA da Meta pode estar ocupando o mesmo lugar que o Coexistence precisa. Não há evidência —
é hipótese. Mas desligá-la é grátis, não apaga nada e leva minutos. **Deve ser tentada antes
de A.**

### Pré-requisito que não cumprimos

O portfólio está com **"Página principal: nenhuma"** — nenhuma Página do Facebook vinculada.
A documentação de troubleshooting lista "Facebook Page Linking Failures" entre as causas de
falha no Coexistence. É o único item da lista de pré-requisitos que não satisfazemos. Os
outros estão ok: app em uso há anos (mínimo são 7 dias), sem selo azul (incompatível com
Coexistence), razão social/endereço/telefone/site preenchidos no portfólio.

---

## Estado do chamado no 360dialog

Conta: Direct Client `93fqW7piCL`, plano **Regular US$ 59/mês** (aprovado por Sostenes),
Visa •2518 cadastrado. Cobrança só acontece quando o número ativa.

Fio de e-mail com `support@360dialog.com`, assunto *"Escalate to your HUMAN technical team,
please. Coexistence QR never..."*, thread `1a0221ec56810ea8`.

| Hora (UTC) | Quem | O quê |
|---|---|---|
| 02:20 | Maria | Lembrete automático de inatividade |
| 02:27 | nós | Mantivemos aberto + 3 perguntas técnicas |
| 02:33 | Maria | Não conseguem ver o BSP; link do doc de migração; *"you are using the incorrect steps"* |
| 02:52 | nós | As duas descobertas + a pergunta bloqueante (remover o cadastro dispara cooldown?) |
| 03:04 | suporte | Pedem **vídeo do fluxo inteiro** + insistem em desconectar a API |
| 03:1x | nós | Prova de que a faixa é a IA da Meta; recusa formal do passo 2; compromisso de gravar o vídeo |

**Pedido em aberto deles:** um vídeo da tentativa, do começo ao fim.
**Pedido em aberto nosso:** o cadastro pendente precisa sair da WABA? Isso dispara cooldown?

---

## O que está pronto e funcionando (não mexer)

- **Site** `site-andreia-carvalho.vercel.app` — pt/en/es, Emily do site (agente ElevenLabs
  próprio, voz nordestina, 3/3 no teste adversarial), formulário de agendamento gravando no
  Supabase, seção de avaliações com aprovação pelo painel, política de privacidade, SEO
  (JSON-LD/canonical/hreflang/sitemap/robots), mobile verificado a 375px.
- **Painel** (`clinic-now-app`) — abas `PedidosSite` e `Depoimentos`, ambas desligadas em modo
  demonstração, com erro explícito de permissão em vez de lista vazia enganosa.
- **Motor** (`medgrowth/emily-vendas`) — `regras.mjs`, `envio.mjs`, `webhook-whatsapp.mjs`
  (HMAC sobre corpo cru, dedup por wamid, fail-closed, opt-out), rota no ar em
  `https://emily-vendas.vercel.app/webhooks/whatsapp`. **190 testes passando.**
- **Faltam 4 variáveis** para o envio real: `WHATSAPP_TOKEN`, `WHATSAPP_PHONE_NUMBER_ID`,
  `WHATSAPP_APP_SECRET`, `WHATSAPP_VERIFY_TOKEN`. Todas dependem do número ativar.

---

## Erros meus nesta linha de trabalho, registrados de propósito

Quem continuar precisa saber onde eu falhei, para não confiar cegamente no que está escrito:

1. **Citação inventada.** Atribuí à Meta uma frase que não existe na documentação. Corrigido
   em `whatsapp-estado-2026-08-17.md` com nota visível, não apagado.
2. **Aceitei a premissa do suporte por horas.** "Já é COEX de outro BSP" era inferência deles
   sobre um banner, e eu tratei como fato — inclusive repetindo para Sostenes como se fosse
   achado da 360dialog.
3. **Recomendei fornecedores sem verificar.** 360dialog sem checar se o cadastro aceitava
   cliente direto; respond.io sem checar que webhook só existe no plano de US$ 279–349.
4. **Deixei respostas do suporte sem ler** — 57 minutos numa, 9 horas noutra. Sostenes precisou
   me avisar.
5. **Duas tentativas de Embedded Signup gastas** antes de ler a documentação de Coexistence
   até o fim. O `Yes, Business app` estava documentado o tempo todo.
