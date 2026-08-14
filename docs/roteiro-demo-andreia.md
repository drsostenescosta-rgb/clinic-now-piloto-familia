# Roteiro da demo — para a Andreia (e depois para a Lohane)

Demonstração de 8 a 10 minutos do ClinicNow com **o nome, os serviços e os preços reais dela**.
Tudo que aparece na tela é sintético: as clientes são "Cliente Demo NN" e nenhuma mensagem veio de
conversa real.

## Antes de sentar com ela

```bash
cd ~/Applications/clinic-now-app && npm run dev
```

<http://localhost:5190> → abre direto na aba **Aprovações**. Não precisa de internet, não precisa
da ponte ligada, não precisa de chave de IA. Se o notebook estiver offline, funciona igual.

Confira no topo: **"Andréia Carvalho Aesthetics · demonstração, sem dados reais"**.

## O que NÃO fazer na demo

- Não prometa envio automático. A Fase 1 não envia — e isso é uma qualidade, não uma limitação.
- Não invente horário. A grade dela ainda está pendente, e a demo mostra a Emily se recusando a
  chutar. Isso é o argumento mais forte que temos.
- Não abra o Financeiro nem a Agenda antes do painel. O painel é a história.

---

## O roteiro

### Abertura (30 s)

> "Mãe, isso aqui não é um robô que responde no seu lugar. É uma fila: a mensagem chega, ele
> escreve o que ele acha que deveria responder, e **você decide**. Nada sai sem você clicar."

Aponte a faixa verde no topo: *Nada é enviado automaticamente.*

### 1. A cliente com pressa (Bruna) — 90 s

Leia a mensagem em voz alta: *"Preciso muito de uma drenagem, tenho um casamento amanhã, dá pra ser hoje??"*

> "Repara no que ele **não** fez: não ofereceu horário nenhum."

Aponte a etiqueta cinza `PEND.2_3_GRADE_HORARIOS` e a linha de bloqueios:
*não afirmar disponibilidade · não oferecer horário específico.*

> "Ele sabe que eu ainda não tenho a sua grade de horários. Então em vez de inventar um horário e
> te deixar na mão, ele anota o que ela quer e devolve pra você. No dia que você me mandar seus
> horários, ele passa a oferecer sozinho."

**Este é o momento-chave da demo.** É o que separa isso de um chatbot.

### 2. A falta na sexta (Larissa) — 90 s

*"Oi, desculpa, não deu pra ir hoje 😔 dá pra remarcar pra semana que vem?"*

> "Sexta é seu horário mais caro. Você me disse que teve 4 faltas em 4 semanas."

Aponte `R5.CANCELAMENTO_TARDIO_SEX_SAB` e *"O horário atual PERMANECE ocupado até a Andreia decidir."*

> "Ele não liberou a vaga, não remarcou, não fez nada. Só te avisou. Porque quem decide dar sexta
> à tarde de graça é você."

Note também a saudação: *"Oi, Larissa! 😘"* — sem se apresentar.
> "Ela já veio 6 vezes. Ele não fica se apresentando pra quem já te conhece."

### 3. A pergunta que parece comercial (Carol) — 60 s

*"Queria saber se o que eu tenho na barriga é flacidez ou gordura localizada, qual tratamento resolve melhor?"*

> "Essa é a pergunta que mais vende — e é a que ele mais tem que calar a boca."

Aponte os bloqueios: *nenhuma indicação de procedimento · nenhum diagnóstico · nenhuma promessa de resultado.*

> "Isso é avaliação sua. Ele não dá palpite no corpo de ninguém."

### 4. O "tá bom" (Jé) — 90 s

*"tá bom 👍"*

> "Quantas vezes alguém te disse 'tá bom' e não apareceu?"

Leia o texto proposto em voz alta:
*"Só para confirmar direitinho: posso considerar confirmado seu horário de drenagem, quarta, 2 da tarde? Responde sim ou não, por favor 😘"*

Três coisas para apontar:
- **"tá bom" não confirmou nada** — o horário segue não confirmado;
- **"drenagem"**, não "drenagem linfática" — é como a cliente fala;
- **"quarta, 2 da tarde"**, não "quarta-feira às 14:00".

> "É a sua frase. Eu só garanti que o dia e a hora saem certos."

### 5. O encaixe (Paty) — 60 s

*"Dá pra me encaixar quinta às 14h? Sei que tem gente nesse horário mas eu preciso muito 🙏"*

Aponte `R2.CONFLITO` e *não mover outra cliente.*

> "Ele nunca vai tirar uma cliente pra colocar outra. Nem se pedirem bonito."

### 6. Aprovar na frente dela — 60 s

Escreva **Andreia** no campo "Quem está aprovando agora". Clique **Aprovar** em qualquer cartão.

> "Aprovei. E olha o que apareceu: *'Mensagem aprovada NÃO foi enviada. Copie e cole no WhatsApp.'*
> Ele nem tem como mandar. Não existe esse botão."

Clique em **copiar texto** e cole no bloco de notas para ela ver.

Se a ponte estiver ligada, mostre o número do registro (`ledger #3`):
> "Ficou registrado: quem aprovou, a que horas, e se mudou alguma palavra. Se um dia alguém
> perguntar 'quem mandou isso?', tem resposta."

### 7. O que falta — 60 s

Aponte a caixa amarela **Preflight REPROVADO** com as 5 pendências.

> "O sistema está se recusando a funcionar de verdade porque faltam 5 respostas suas. Ele não
> chuta. As duas que travam tudo são: **seus horários da semana**, em formato de 24 horas, e
> **três mensagens escritas do seu jeito** — uma pra quem pergunta preço, uma pra quem quer
> horário, e uma pra quem quer falar direto com você."

> "Me manda essas duas e na semana que vem ele começa a oferecer horário sozinho."

### Fechamento (30 s)

> "Semana que vem eu aprovo por você, você só atende. Na outra, você assume o painel — são três
> paradas de dez minutos por dia: manhã, intervalo e fim do dia."

---

## Para a Lohane (próxima clínica)

Mesmo roteiro, com uma troca de enquadramento na abertura:

> "Isso aqui é o que rodou na clínica da minha mãe primeiro. Os serviços e os preços na tela são os
> dela; no seu caso a gente troca por um arquivo de configuração e o sistema inteiro passa a falar
> a sua língua."

E acrescente no fim, porque é o diferencial comercial:

> "O que trava aqui não é tecnologia. É a resposta que a dona não deu. Enquanto não deu, o sistema
> se recusa a inventar. É por isso que ele não vai te queimar com uma cliente."

**Não mostre** o repositório, o código nem o terminal para a Lohane. Só o painel.
