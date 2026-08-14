# Fonte de verdade da agenda: Agendor

- **Data:** 2026-08-14
- **Status:** ✅ **DECIDIDA — opção C (Agendor manda, ClinicNow espelha).** Implementada atrás de flag em `~/Applications/medgrowth/emily-vendas/agendor.mjs`. Falta **só o token da Andreia** para ligar. Ver "Resolução" no fim.

## Contexto

O PRD (RF-04) exige **uma única** agenda declarada como fonte de verdade. Na resposta 2.1 do questionário, Andreia declarou: *"Minha agenda Agendor"*.

Isso foi gravado em `config/agenda-config.json` (`fonte_verdade: "Agendor"`), porque é o que ela respondeu — não cabia a nós escolher outra.

## O problema que isso cria

O ClinicNow (`~/Applications/clinic-now-app`) tem agenda própria, em Supabase, com prevenção determinística de conflito. Se a fonte de verdade é o Agendor, existem duas agendas e a do app **não** decide nada. Ou seja:

- a Emily não consegue afirmar que um horário está livre sem ler o Agendor;
- a prevenção de conflito do app deixa de ser garantia real e vira segunda opinião;
- toda proposta de horário exige um humano conferindo o Agendor à mão — que é justamente o trabalho manual que o piloto quer eliminar.

## Opções

**A. Agendor continua a fonte de verdade; o app só propõe.**
Fiel à resposta dela e ao fluxo que ela já usa. Custo: sem integração com o Agendor, cada proposta de horário exige conferência manual. Precisa investigar se o Agendor tem API utilizável.

**B. A agenda do app vira a fonte de verdade; o Agendor sai de cena.**
Dá ao piloto a prevenção de conflito de verdade e o ganho de tempo prometido. Custo: obriga Andreia a mudar de ferramenta no meio do piloto — risco alto de rejeição, e ela perde o histórico dela.

**C. Agendor como fonte de verdade + espelho de leitura no app.**
Meio-termo. Depende inteiramente de o Agendor expor API. Sincronização divergente já tem regra definida (resposta 2.7: pausar e perguntar).

## Recomendação do executor

Não decidir antes de responder uma pergunta factual: **o Agendor expõe API de leitura de agenda?**

- Se sim → opção **C**.
- Se não → opção **A** para a Fase 1 (o ganho do piloto vem da velocidade de resposta, não do agendamento automático), e reabrir a discussão da opção B só depois que ela sentir valor.

Trocar a ferramenta da mãe na primeira semana é o caminho mais rápido para o piloto morrer.

## Consequência se ficar sem decisão

A Emily fica proibida de afirmar disponibilidade (já está assim no prompt atual). Ela coleta preferência de horário e devolve para conferência humana.

---

## Resolução — 14/08/2026

**Escolhida a opção C.** O Agendor continua sendo a fonte de verdade; o ClinicNow lê e escuta.
Trocar a ferramenta da Andreia na primeira semana continua sendo o caminho mais rápido de matar o
piloto, e agora não é preciso: o espelho de leitura está implementado.

### O que foi verificado de fato

| Item | Estado |
|---|---|
| Autenticação `Authorization: Token <uuid>` | ✔ confirmado na documentação pública |
| Base da API v3 `https://api.agendor.com.br/v3` | ✔ confirmado |
| Assinatura de webhook: `POST /integrations/subscriptions` com `{target_url, event}`, evento `on_activity_created` | ✔ confirmado |
| **Caminho exato de LEITURA de compromissos e formato da resposta** | ✘ **NÃO verificado** — impossível sem um token real |

Chutar o endpoint seria exatamente a suposição que este piloto proíbe. Por isso existe um comando
de descoberta em vez de um palpite:

```bash
cd ~/Applications/medgrowth
AGENDOR_TOKEN=<token-da-andreia> npm run agendor:descobrir
```

Ele sonda os seis candidatos (`/tasks`, `/users/tasks`, `/deals/tasks`, `/activities`, `/events`…),
reporta o status de cada um e grava o que respondeu 200 em `.agendor/descoberta.json`. Só depois
disso a leitura liga.

### O que falta, exatamente

1. **Token da API.** A Andreia gera em Agendor → Configurações → Integrações → API. É um dado dela;
   Sostenes não deve gerar por ela nem guardar no repositório — vai no `.env` local.
2. **Confirmar que o plano dela libera a API.** Se `descobrir` devolver 403 em tudo, o plano não
   libera e a opção C morre — aí a Fase 1 segue na opção A (a Emily não afirma disponibilidade,
   que já é o comportamento atual e passa nos testes).
3. **`AGENDOR_WEBHOOK_SECRET`** e uma URL pública para o webhook, quando quisermos tempo real.
   Sem isso o espelho é sincronizado por comando (`npm run agendor:sync`).

### Proteções que já estão no código

- **Desligado por padrão.** Sem `AGENDOR_ENABLED=true` + token, o módulo devolve
  `{disponivel:false, motivo}` e **não trava o painel** — a operação segue sem afirmar horário.
- **Espelho velho não vale.** Acima de 15 minutos o espelho é marcado `obsoleto` e a ponte trata
  como indisponível. Dado velho de agenda é pior que dado ausente: gera dupla marcação.
- **Somente leitura.** Não existe função que escreva na agenda dela. A escrita continua sendo dela,
  no Agendor.
- **Webhook exige HMAC.** POST sem `X-ClinicNow-Signature` válido devolve 401 — verificado.
- **Item sem data é descartado**, nunca convertido em horário chutado.

