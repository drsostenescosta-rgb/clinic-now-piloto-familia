# ClinicNow — Piloto Família

Primeiro caso de aplicação real do **ClinicNow** (o produto único para clínicas — arquitetura em `~/Applications/clinic-now-access/docs/ARQUITETURA_UNIFICACAO.md`): o atendimento da clínica da **mãe de Sostenes**.

## O que é este piloto

- **Problema:** a clínica perde 2–3 dias/semana de trabalho em agendamento manual pelo WhatsApp (fato validado na mentoria de 07/08/2026), com leads perdidos por demora e no-shows por falta de confirmação.
- **Prova de valor:** a Emily (assistente do ClinicNow) responde e agenda pelo WhatsApp da clínica, em modo assistido, e a dona recebe agendamentos confirmados **sem precisar olhar o celular** durante os atendimentos.
- **Por que a mãe primeiro:** acesso total, feedback honesto, risco comercial zero — decisão da mentoria de 07/08. Tudo que ela validar vira o template de onboarding de qualquer clínica futura.

## Estado atual

Entrevista de descoberta realizada em 11/08/2026 e dores operacionais registradas. O consentimento assinado e as respostas bloqueadoras do questionário ainda precisam ser confirmados antes de qualquer uso de dado real.

Já existem duas entregas independentes dessas respostas:

- um workbench local de shadow sintético, com cinco cenários críticos e revisão independente aprovada somente para simulação;
- uma fundação local do app em modo sintético, com aliases artificiais, bloqueio de integrações, buffer e prevenção de sobreposição. Auth/RLS e SQL estão apenas preparados para aplicação futura, não executados.

O estado detalhado, os comandos e os limites estão em `docs/estado-implementacao-2026-08-11.md`.

**14/08:** as três entregas acima foram ligadas ponta a ponta. O motor de conversa que já existia
em `~/Applications/medgrowth/emily-vendas` ganhou um **motor de regras determinístico**, um
**ledger de aprovações** e uma **ponte HTTP**; o ClinicNow ganhou o **Painel de Aprovação**, que é
a tela onde a dona realmente trabalha na Fase 1. Nada é enviado automaticamente. Como operar:
`docs/kit-operacao-diaria.md`. Como demonstrar: `docs/roteiro-demo-andreia.md`.

## Regras do piloto (invioláveis)

1. **Dados dela ficam locais** neste repositório e/ou na conta Supabase da operação — nada é publicado, nada vira material de marketing sem novo consentimento específico.
2. **Modo assistido primeiro**: um humano (Sostenes) revisa/envia toda mensagem no ciclo 1; a Emily redige, o humano aprova.
3. **Zero orientação clínica, zero desconto, zero promessa** pela IA — fora do escopo = escala para humano.
4. API não-oficial de WhatsApp é **vetada** (risco de banimento do número real da clínica).
5. Gravações e anotações só com o consentimento de `docs/consentimento.md` assinado antes.

## Mapa do repositório

- `config/clinica-andreia.md` — **configuração da clínica** a partir das respostas de Andreia (11/08/2026): identidade, agenda, serviços, regras invioláveis e as pendências abertas. Comece por aqui.
- `config/clinica-config.json`, `config/agenda-config.json`, `config/operacao-assistida.json` — a mesma configuração no formato que o preflight lê.
- `docs/kit-operacao-diaria.md` — **como rodar o dia**: quem aprova, em que horário, o que fazer quando a Emily escala, e as métricas de base. É o documento que a Andreia usa.
- `docs/roteiro-demo-andreia.md` — roteiro da demonstração de 10 minutos (serve também para a Lohane).
- `docs/roteiro-entrevista.md` — roteiro da entrevista de descoberta com a mãe.
- `docs/consentimento.md` — termo de consentimento LGPD para gravação e uso dos dados no piloto.
- `docs/operacao-assistida-andreia.md` — runbook da operação humana no loop.
- `docs/estado-implementacao-2026-08-11.md` — entregas implementadas, evidências e gates ainda abertos.
- `BACKLOG.md` — as fatias do piloto, em ordem.
- `docs/decisoes/` — decisões caras de reverter, uma por arquivo.
