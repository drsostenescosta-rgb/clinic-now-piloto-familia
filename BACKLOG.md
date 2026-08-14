# Backlog — clinic-now-piloto-familia

Fonte única de verdade do piloto. Mantido por Sheldon. Criado em 2026-08-08.

Regra do piloto: cada fatia termina com algo que a mãe **vê funcionando** no atendimento dela. Nada de infraestrutura invisível.

## Agora (esta semana)

- [ ] E1. **Fechar a descoberta com a mãe** — questionário respondido em 11/08/2026 e transformado em configuração real em `config/` (14/08). **Faltam 5 respostas** para o preflight passar: horários semanais (AM/PM ambíguo e conflitando com a academia), 3 respostas-modelo no tom dela, exemplos anonimizados, retenção de registros e regra de sinal/lista de espera. Consentimento assinado continua pendente. — valor: é a matéria-prima de tudo — o system prompt da Emily nasce da operação real, não de suposição. — esforço: P (só as 5 pendências) — **prioridade 1 (dono: Sostenes)**
- [ ] E2. **Mapear o fluxo de atendimento atual** — desenhar em uma página (docs/) o caminho real: cliente chama → alguém responde → marca → confirma → atende → retorno; marcar onde o tempo se perde e onde lead escapa. — valor: mostra para a mãe (e para nós) exatamente o que o sistema vai tirar das costas dela; define a métrica base (horas/semana em agendamento manual). — esforço: P — **prioridade 2 (dono: Sostenes + Sheldon)**
- [ ] E3. **Configurar o primeiro módulo útil: Emily assistida no WhatsApp** — WhatsApp Business no número da clínica (perfil, horário, saudação); Sheldon escreve o system prompt v1 a partir da E1; ciclo 1 em modo assistido (Emily redige, Sostenes revisa e envia, resposta < 1 min na janela definida); cada agendamento do dia registrado no Supabase (`clinicnow_*`). — valor: a primeira semana em que a mãe recebe agendamentos confirmados sem olhar o celular — a prova de valor do piloto inteiro. — esforço: M — **prioridade 3 (dono: Sostenes opera, Sheldon prepara)**

## Próximo

- [ ] P1. **Suite de avaliação do prompt** — 20–30 casos reais (da entrevista + primeiras conversas) com resposta esperada; casos de segurança (pergunta clínica, desconto, emergência) passam 100% antes de qualquer automação. — valor: mudança de prompt vira coisa testável, não achismo.
- [ ] P2. **Confirmação de véspera anti no-show** — Emily (assistida) confirma os horários de amanhã; "não posso" abre remarcação. — valor: ataca a dor nº 1 do segmento com o que já está montado.
- [ ] P3. **Revisão do ciclo 1 com a mãe** — sentar com ela: o que ajudou, o que atrapalhou, o que a Emily errou; decidir juntos se liga a automação (WhatsApp Cloud API). — valor: feedback real > produto perfeito; é o gate honesto para a fase 2 do produto único.

## Depois (não refinado)

- Automação do webhook WhatsApp (depende da verificação Meta, iniciada no backlog do produto).
- Painel simples para ela ver a agenda do dia.
- Novo consentimento específico se algum aprendizado do piloto for virar material público.

## Feito

- [x] E0. Scaffold do repositório do piloto: README, CLAUDE.md, roteiro de entrevista, termo de consentimento LGPD, backlog (2026-08-08) — aprendizado: o piloto já nasce com a LGPD resolvida antes do primeiro dado coletado.
- [x] F0. Runbook e preflight da operação assistida, com envio automático bloqueado e revisão humana obrigatória (2026-08-11).
- [x] F1. Workbench de shadow 100% sintético: cinco cenários, duração + buffer, fuso/faixa, PII fail-closed e relatório com hash; 27/27 testes e terceira revisão independente aprovada somente para shadow (2026-08-11).
- [x] F3. Configuração real da clínica a partir das respostas de Andreia (2026-08-14): `config/clinica-andreia.md` + os 3 JSON do contrato do preflight; catálogo real (3 serviços) aplicado no Supabase `sosmed` com os 7 serviços fictícios desativados; prompt da Emily do ClinicNow reescrito com as regras dela. Preflight **reprova com 11 problemas**, todos nas 5 pendências — comportamento correto (fail-closed). Aprendizado: as pendências foram escritas como `[PREENCHER` para reusar o detector que o preflight já tinha, em vez de inventar um segundo mecanismo.
- [x] F2. Fundação local do app em modo sintético: aliases artificiais, integrações externas desativadas, snapshots de duração/buffer e anti-overbooking; 6/6 testes e builds synthetic/owner fail-closed. SQL/Auth/RLS real permanece não aplicado e não verificado (2026-08-11).
