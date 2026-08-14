# Fonte de verdade da agenda: Agendor

- **Data:** 2026-08-14
- **Status:** ⚠️ **ABERTA — aguardando decisão de Sostenes.** A configuração já grava `Agendor`, mas a consequência abaixo não foi resolvida.

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
