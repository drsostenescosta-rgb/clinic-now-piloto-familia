# Como usar o painel na nuvem

**Escrito em 15/08/2026.** Substitui o trecho do kit de operação que mandava subir servidor no Mac.

## O endereço

**https://clinic-now-app.vercel.app**

Funciona no celular, no navegador, de qualquer lugar. **Não depende do Mac do Sostenes estar
ligado** — era o problema que isto resolve.

## Como a Andreia entra

1. Abre o endereço.
2. Digita o e-mail dela.
3. Recebe um link no e-mail e clica. Pronto — sem senha.

O link mágico **não cria conta**. Só quem já foi cadastrado recebe. Se o e-mail dela não estiver
cadastrado, ela pede o link, nada chega, e ninguém entra. É de propósito: sem isso, qualquer
pessoa que descobrisse o endereço viraria usuária.

**Falta cadastrar a Andreia.** O Sostenes já está. Para incluir alguém é preciso o e-mail dela —
uma linha de SQL no Supabase, feita uma vez.

Sugestão prática: fixar o site na tela inicial do celular dela (Safari → Compartilhar → "Adicionar
à Tela de Início"). Vira ícone, abre como aplicativo.

## O ciclo de trabalho dela

1. Chega mensagem no WhatsApp.
2. Ela copia, abre o painel, cola no campo "O que ela escreveu".
3. Toca em **"Ver o que a Emily propõe"**.
4. Lê o rascunho. **Aprova, edita ou escala.**
5. Toca em **"copiar texto"** e cola no WhatsApp.

**Aprovar não envia nada.** O envio é sempre ela, no WhatsApp dela. O painel repete isso na tela
porque essa é a promessa da Fase 1.

## O que fica registrado

Toda proposta e toda decisão entram num registro que **não pode ser alterado nem apagado** — o
banco recusa `UPDATE` e `DELETE`. Cada evento carrega o código do anterior, então mexer em um
quebra a corrente inteira e a quebra aparece na tela.

O nome de quem aprovou **vem do login**, não do campo digitado. Testado: digitei "Impostora" no
campo e o registro gravou o nome real da conta.

Só a Andreia e o Sostenes leem esses registros — é a resposta 7.2 dela virada regra de banco.
Qualquer outra conta, mesmo logada, lê zero.

## O que ainda não está lá

- **A Andreia não tem login** até o e-mail dela ser informado.
- **O WhatsApp não está conectado.** Ela copia e cola. Foi decisão de 15/08: primeiro ela usa,
  depois se decide o número oficial da Meta.
- **A purga dos 90 dias é manual.** A política está definida; o apagamento automático ainda não
  foi escrito.
- **Duas pendências dela** continuam abertas e aparecem na tela do painel: as três frases no tom
  dela (6.5) e os seis exemplos anonimizados (9.1/9.2).

## Onde cada coisa mora

| Peça | Onde | Observação |
|---|---|---|
| Painel | `clinic-now-app.vercel.app` | Vercel, projeto `clinic-now-app` |
| Ponte (motor + regras) | `emily-vendas.vercel.app` | Vercel, projeto `emily-vendas`; sem rota de envio |
| Registro | Supabase `sosmed`, tabelas `andreia_*` | append-only, RLS por allowlist |
| Regras e configuração | `clinic-now-piloto-familia/config` | fonte única; `npm run config:sync` leva para a nuvem |

**Ao mudar qualquer configuração:** rodar `npm run config:sync` em `emily-vendas` e publicar de
novo. Sem isso a nuvem opera com a configuração velha — e a tela não avisa.

## Se der problema

- **"sua conta não está autorizada"** → a conta existe mas não está na allowlist do banco.
- **O link não chega** → o e-mail não está cadastrado. Link mágico não cria conta.
- **A tela diz que não consegue falar com a ponte** → ver `emily-vendas.vercel.app/api/estado`.
  Sem login, tem que responder `401`. Se responder outra coisa, o problema é na ponte.
