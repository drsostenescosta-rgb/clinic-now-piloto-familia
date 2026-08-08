# ClinicNow — Piloto Família

Primeiro caso de aplicação real do **ClinicNow** (o produto único para clínicas — arquitetura em `~/Applications/clinic-now-access/docs/ARQUITETURA_UNIFICACAO.md`): o atendimento da clínica da **mãe de Sostenes**.

## O que é este piloto

- **Problema:** a clínica perde 2–3 dias/semana de trabalho em agendamento manual pelo WhatsApp (fato validado na mentoria de 07/08/2026), com leads perdidos por demora e no-shows por falta de confirmação.
- **Prova de valor:** a Emily (assistente do ClinicNow) responde e agenda pelo WhatsApp da clínica, em modo assistido, e a dona recebe agendamentos confirmados **sem precisar olhar o celular** durante os atendimentos.
- **Por que a mãe primeiro:** acesso total, feedback honesto, risco comercial zero — decisão da mentoria de 07/08. Tudo que ela validar vira o template de onboarding de qualquer clínica futura.

## Estado atual

Fase de descoberta. Próximo passo: Sostenes entrevista a mãe com o roteiro de `docs/roteiro-entrevista.md`, colhe o consentimento de `docs/consentimento.md`, e o resultado alimenta o system prompt v1 da Emily.

## Regras do piloto (invioláveis)

1. **Dados dela ficam locais** neste repositório e/ou na conta Supabase da operação — nada é publicado, nada vira material de marketing sem novo consentimento específico.
2. **Modo assistido primeiro**: um humano (Sostenes) revisa/envia toda mensagem no ciclo 1; a Emily redige, o humano aprova.
3. **Zero orientação clínica, zero desconto, zero promessa** pela IA — fora do escopo = escala para humano.
4. API não-oficial de WhatsApp é **vetada** (risco de banimento do número real da clínica).
5. Gravações e anotações só com o consentimento de `docs/consentimento.md` assinado antes.

## Mapa do repositório

- `docs/roteiro-entrevista.md` — roteiro da entrevista de descoberta com a mãe.
- `docs/consentimento.md` — termo de consentimento LGPD para gravação e uso dos dados no piloto.
- `BACKLOG.md` — as fatias do piloto, em ordem.
- `docs/decisoes/` — decisões caras de reverter, uma por arquivo.
