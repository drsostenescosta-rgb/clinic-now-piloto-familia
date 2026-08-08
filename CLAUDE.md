# CLAUDE.md — clinic-now-piloto-familia

Contexto para agentes trabalhando neste repositório.

## O que é

Piloto real nº 1 do ClinicNow: aplicar o sistema no atendimento da clínica da mãe de Sostenes. Documentos e dados operacionais do piloto — ainda não há código aqui.

## Referências obrigatórias

- Arquitetura do produto único: `~/Applications/clinic-now-access/docs/ARQUITETURA_UNIFICACAO.md`
- PRD e backlog da v1 (Emily agendamento WhatsApp): `~/Applications/medgrowth/docs/PRD-clinicnow.md` e `BACKLOG-clinicnow.md`
- Backend/Supabase já provisionado: `~/Applications/farol/ESPECIFICACAO-BACKEND.md` (projeto `sosmed`, tabelas `clinicnow_*` prontas)

## Regras invioláveis

- **Dados da mãe e de pacientes dela são dados sensíveis (art. 11 LGPD)**: nunca colar em documentos versionados nomes de pacientes, transcrições brutas ou qualquer dado identificável. Dados operacionais reais vivem no Supabase com RLS, não no git.
- Nenhuma gravação sem o termo de `docs/consentimento.md` assinado.
- Modo assistido no ciclo 1: IA redige, humano envia.
- Não usar API não-oficial de WhatsApp (vetado — risco ao número da clínica).
- Decisão cara de reverter → um arquivo em `docs/decisoes/` com data, contexto, opções, escolha e porquê.

## Convenções

- Idioma: português. Linguagem simples — a leitora final dos roteiros é a mãe, não um engenheiro.
- Backlog no formato padrão de Sheldon (fatias verticais com valor de negócio) em `BACKLOG.md`.
