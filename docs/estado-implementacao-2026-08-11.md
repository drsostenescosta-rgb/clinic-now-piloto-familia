# Estado da implementação independente — 11/08/2026

## Decisão

Foi implementado apenas o que não depende das respostas finais de Andreia e não exige conta, credencial, canal ou dado real. O objetivo desta etapa é tornar as regras verificáveis antes de configurar a operação verdadeira.

## 1. Operação assistida

Runbook: `docs/operacao-assistida-andreia.md`.

- humano revisa toda resposta e toda alteração;
- “tá bom”, emoji, silêncio ou resposta genérica não confirmam nada;
- conflito, cancelamento, dúvida clínica e exceção comercial pausam e escalam;
- nenhuma mensagem, reativação, oferta ou conteúdo é enviado automaticamente.

## 2. Workbench de shadow sintético

Repositório: `/Users/sostenesdaschagascostajrpaiva/Applications/medgrowth/emily-vendas`.

Comando:

```bash
cd /Users/sostenesdaschagascostajrpaiva/Applications/medgrowth
npm run shadow:workbench
npm test
```

Estado verificado:

- exatamente cinco cenários: novo pedido, conflito, confirmação ambígua, cancelamento ambíguo e escalação clínica;
- horários consideram duração, buffer, fuso, dia e faixa ativa;
- PII, texto livre fora do schema e cenários duplicados falham fechados;
- relatório nasce com revisão humana pendente e nunca autoriza go-live;
- 27/27 testes passaram; build passou; terceira revisão independente não encontrou P0/P1 no escopo sintético.

## 3. Fundação local do app

Repositório: `/Users/sostenesdaschagascostajrpaiva/Applications/clinic-now-app`.

Comando seguro:

```bash
cd /Users/sostenesdaschagascostajrpaiva/Applications/clinic-now-app
npm run dev:synthetic
```

Estado verificado:

- usa apenas aliases `Paciente Demo NN` e remove armazenamento legado/inválido;
- WhatsApp, Instagram, Google Agenda e ElevenLabs permanecem desativados;
- não existem botões para forçar conflito;
- agenda usa snapshots de duração e buffer;
- SQL de contenção, backfill e finalização está staged fora de migrations automáticas;
- 6/6 testes passaram; builds synthetic e owner fail-closed passaram; terceira revisão independente aprovou somente a fundação local sintética.

## O que ainda depende de Andreia

- nome/endereço público e idiomas;
- agenda que será a fonte de verdade;
- horários, pausas e bloqueios fixos;
- 3 a 5 serviços iniciais, com preço, duração e buffer de cada um;
- confirmação, atraso, remarcação, cancelamento, no-show e lista de espera;
- responsável, canal e prazo de escalação;
- tom de voz e textos aprovados;
- consentimento, acesso e retenção;
- exemplos anonimizados e métricas de base.

## Gates que continuam fechados

- não aplicar os SQLs staged nem habilitar modo owner sem backup, respostas aprovadas, usuário Auth e matriz RLS positiva/negativa;
- não usar nomes, telefones, conversas, anamnese ou qualquer dado real;
- não conectar WhatsApp, Instagram, Google Agenda, ElevenLabs ou Supabase real;
- não chamar o shadow de piloto real: a revisão humana do relatório continua pendente.

## Limite de verificação desta rodada

Não havia Postgres, Supabase CLI ou Docker local; portanto SQL, RLS e concorrência no banco não foram executados. A tentativa de abrir o preview local foi bloqueada pelo ambiente, embora testes e builds tenham passado.
