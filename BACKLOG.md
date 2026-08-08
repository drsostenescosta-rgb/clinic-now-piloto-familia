# Backlog — clinic-now-piloto-familia

Fonte única de verdade do piloto. Mantido por Sheldon. Criado em 2026-08-08.

Regra do piloto: cada fatia termina com algo que a mãe **vê funcionando** no atendimento dela. Nada de infraestrutura invisível.

## Agora (esta semana)

- [ ] E1. **Entrevista de descoberta com a mãe** — Sostenes conduz com `docs/roteiro-entrevista.md`; antes de gravar, colher assinatura de `docs/consentimento.md`; depois, rodar o checklist do fim do roteiro (procedimentos, preços, agenda, FAQs, lista do "nunca responder"). — valor: é a matéria-prima de tudo — o system prompt da Emily nasce da operação real, não de suposição. — esforço: M — **prioridade 1 (dono: Sostenes)**
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
