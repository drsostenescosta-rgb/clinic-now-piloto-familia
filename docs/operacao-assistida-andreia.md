# Kit de Operação Assistida — Andreia Carvalho

**Estado:** pré-go-live. Este documento orienta um piloto interno; não autoriza envio automático, conexão de WhatsApp/Instagram/Google Calendar, nem o uso de dados reais de pacientes em LLM.

## Objetivo da primeira fatia

Reduzir o tempo de resposta sem transferir decisão operacional ou clínica para a IA. A Emily pode preparar rascunhos apenas com exemplos sintéticos/redigidos no shadow após o preflight verde. Uma pessoa designada revisa cada resultado; nenhum canal real é acionado nesta fatia.

## Limites inegociáveis

- Nunca marcar duas pessoas no mesmo horário.
- Nunca cancelar, remover ou mover um horário sem confirmação explícita da cliente e revisão humana.
- “Tá bom”, “ok”, emoji ou silêncio **não** são confirmação. Perguntar novamente qual ação a cliente autoriza.
- Perguntas pessoais, capciosas, clínicas, urgentes, sobre contraindicação, preço não autorizado ou fora do catálogo vão para Andreia.
- A operação não faz promessa de resultado clínico, corporal ou financeiro. Benefícios, calorias, resultados e preços só podem usar material aprovado pela clínica e, quando aplicável, evidência do fabricante/profissional responsável.
- Não colar conversas reais, nomes, telefones, imagens, diagnósticos, procedimentos ou pagamentos em repositório, Notion ou prompts. Nesta fatia, dados reais não entram na Emily/LLM. Somente exemplos sintéticos ou redigidos sem qualquer dado identificável ou clínico podem ser usados na simulação shadow.
- Não há disparo automático de reativação, lembrete, upsell ou conteúdo nesta fase.

## Fluxo humano no loop

1. A mensagem chega no canal da clínica; Andreia/Sostenes a classifica como nova cliente, cliente recorrente, pedido de horário, confirmação, cancelamento ou escalonamento.
2. Nesta fatia, o operador **não envia a mensagem real à Emily**. Ele cria um exemplo sintético ou uma abstração redigida, sem identificadores nem dados clínicos, e usa somente esse texto para gerar um rascunho. Depois revisa tom, factualidade e se há motivo de escalação.
3. Para sugerir horário, o operador consulta a agenda configurada. O sistema deve respeitar duração e buffer do serviço.
4. A cliente escolhe um horário. O operador pede confirmação explícita com data, hora e serviço.
5. Só depois da resposta explícita, o operador grava o agendamento e confere novamente conflito de agenda.
6. Na véspera, o operador envia um lembrete humanizado e registra a resposta. Ausência de resposta não vira confirmação.
7. Cancelamento, remarcação, dúvida sensível ou falha de agenda: pausar automação e escalar para Andreia.

## Preflight obrigatório

Antes de qualquer rodada assistida, executar:

```bash
cd /Users/sostenesdaschagascostajrpaiva/Applications/medgrowth/emily-vendas
node preflight.mjs
```

O comando falha se houver placeholder em qualquer configuração, responsável de escalação ausente, regras de confirmação/cancelamento/transparência ausentes, envio automático liberado, revisão humana ausente, tópico de escalação ausente, serviço sem política operacional ou configuração inválida de duração, buffer, fonte de verdade, fuso e faixas de horário. Um resultado verde libera **somente** a preparação do shadow sintético revisado por humano: não comprova que as simulações foram executadas e não autoriza dados reais, conexão com WhatsApp/calendário/banco ou go-live.

## Contrato de configuração operacional

O preflight espera um arquivo local `operacao-assistida.json` (não versionar se contiver dados identificáveis), com esta forma mínima:

```json
{
  "modo": "assistido",
  "responsavel_escalacao": "Nome da responsável",
  "automacao": { "envio_automatico": false },
  "revisao_humana_obrigatoria": true,
  "escalonamento": {
    "topicos": ["clinico", "urgencia", "pessoal", "preco_nao_autorizado"]
  },
  "shadow_sintetico": {
    "habilitado": true,
    "revisor_humano": "Nome da responsável",
    "simulacoes": ["novo_pedido", "conflito", "confirmacao_ambigua", "cancelamento", "escalacao_clinica"]
  },
  "regras": {
    "confirmacao": {
      "exige_confirmacao_explicita": true,
      "reconfirma_termos_ambiguos": true
    },
    "cancelamento": {
      "nao_remove_sem_confirmacao_explicita": true,
      "revisao_humana_obrigatoria": true
    },
    "transparencia": {
      "identifica_como_assistente": true
    }
  },
  "servicos": [
    {
      "id": "avaliacao",
      "politica_operacional": {
        "duracao_min": 60,
        "buffer_min": 10,
        "requer_confirmacao_explicita": true,
        "escalar_se_duvida": true
      }
    }
  ]
}
```

O arquivo acima é um molde, não um cadastro de pacientes nem autorização para automação. A responsável deve completar e aprovar as políticas por serviço após responder o questionário de descoberta.

Na configuração da agenda, informar ainda `buffer_min`, `fonte_verdade` e `fuso_horario` IANA (por exemplo, `America/New_York`), além de ao menos uma faixa ativa. A fonte de verdade deve ser conferida pelo revisor humano em cada simulação; o preflight não integra calendários.

## Critério de saída desta fatia

- Preflight verde com dados operacionais aprovados pela Andreia, revisor humano nomeado e as cinco simulações obrigatórias declaradas no contrato: `novo_pedido`, `conflito`, `confirmacao_ambigua`, `cancelamento` e `escalacao_clinica`.
- Execução separada das cinco simulações, com resultado e decisão do revisor registrados fora do Git. A presença dos nomes na configuração não prova essa execução.
- Registro de quem revisará mensagens e agenda em cada período de atendimento.
- Consentimento aplicável e decisão explícita antes de conectar qualquer canal ou dado real. O verde desta fatia, isoladamente, nunca é autorização de go-live.
