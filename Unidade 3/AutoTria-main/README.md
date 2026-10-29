# AutoTria · Triagem Automática de Mensagens de Contato com IA

Projeto da disciplina **Tendências em Ciência da Computação** (Unidade 3 – Low Code / No Code / Vibecode).

Cada mensagem enviada pelo formulário "Fale Conosco" é validada, confirmada por e-mail ao remetente e classificada por IA em **categoria**, **prioridade** e **resumo**, tudo registrado numa tabela do Airtable, sem nenhuma linha de código de integração.

## 1. Desafio e objetivo

**Problema.** Pequenos negócios recebem mensagens pelo formulário de contato e precisam ler, classificar e responder cada uma manualmente. Isso gera demora, mensagens esquecidas e nenhuma visão organizada dos pedidos.

**Público.** Responsáveis pelo atendimento e, indiretamente, os clientes, que passam a receber confirmação imediata.

**Objetivo.** Automatizar a triagem com IA: categoria (`duvida`, `reclamacao`, `orcamento`, `outro`), prioridade (`baixa`, `media`, `alta`), resumo de uma linha, registro na tabela e e-mail de confirmação.

**Fora do escopo.** Respostas redigidas pela IA, chat em tempo real, WhatsApp, CRM, login e anexos.

**Critérios de sucesso.**

1. 100% das mensagens válidas registradas.
2. Pelo menos 80% das classificações de teste corretas.
3. E-mail de confirmação entregue em até 15 minutos.
4. Entradas inválidas bloqueadas antes da IA.
5. Falhas da IA registradas como "Revisar manualmente", sem travar o fluxo.

## 2. Plataforma e justificativa

| Peça | Ferramenta | Por quê |
| --- | --- | --- |
| Formulário e banco de dados | Airtable (plano gratuito) | Formulário nativo e tabela visual; 1.000 registros por base |
| Automação | Make (plano gratuito) | Único plano gratuito com fluxo de várias etapas, módulos de IA e tratamento de erros; 1.000 créditos/mês, cerca de 160 mensagens válidas (medido nos testes) |
| IA | Make AI Toolkit · Simple Text Prompt | Usa o provedor de IA nativo do Make, sem chave de API externa |
| E-mail | Gmail | Gratuito e com módulo pronto no Make |

Alternativas descartadas: Zapier (plano gratuito com 100 tarefas/mês e sem fluxos de várias etapas), automações do próprio Airtable (100 execuções/mês), Bubble e Adalo (voltados a apps completos, mais do que o desafio exige).

## 3. Arquitetura do fluxo

```text
Formulário Airtable
      │
      ▼
[1] Airtable · Watch records (novos registros, Status vazio)
      │
      ▼
[2] Router
      ├── Rota "Dados válidos" (e-mail com @ e . ; mensagem de 10 a 2.000 caracteres)
      │     [3] Gmail · Send an email ............ confirmação com texto fixo
      │     [4] AI Toolkit · Simple Text Prompt .. classifica e devolve JSON
      │     [5] JSON · Parse JSON
      │     [6] Airtable · Update a record ....... Categoria, Prioridade, Resumo, Status = Triado
      │     (erro em 4, 5 ou 6) → Airtable · Update a record: Status = Revisar manualmente
      │
      └── Rota "Dados inválidos"
            [9] Airtable · Update a record ....... Status = Inválido (IA não é chamada)
```

O e-mail de confirmação sai **antes** da IA, então o remetente é atendido mesmo se a classificação falhar.

![Fluxo do cenário](docs/diagrams/fluxo.png)

## 4. Como configurar

1. **Airtable.** Crie a base `Triagem de Contatos` com a tabela `Contatos` e os campos:

   | Campo | Tipo |
   | --- | --- |
   | Nome | Texto curto (campo principal) |
   | Email | E-mail |
   | Mensagem | Texto longo |
   | Categoria | Seleção única: duvida, reclamacao, orcamento, outro |
   | Prioridade | Seleção única: baixa, media, alta |
   | Resumo | Texto longo |
   | Status | Seleção única: Novo, Triado, Inválido, Revisar manualmente |
   | Data | Hora de criação |

   Crie um formulário com Nome, Email e Mensagem (todos obrigatórios) e publique-o.
2. **Make.** Crie um cenário e importe `config/blueprint.json` (menu **…** › *Import Blueprint*).
3. Conecte o **Airtable**, o **Gmail** e o **Make's AI Provider** nos módulos indicados (no módulo de IA, modelo *Medium*). Selecione a base e a tabela no módulo 1.
4. Ative o cenário. No plano gratuito, ele verifica novos registros a cada 15 minutos.

## 5. Como usar

1. Envie uma mensagem pelo formulário público.
2. Em até 15 minutos o remetente recebe o e-mail de confirmação.
3. Na tabela `Contatos`, o registro aparece com Categoria, Prioridade, Resumo e Status preenchidos. Filtre por `Prioridade = alta` para atender primeiro o que é urgente.

## 6. IA e prompts

O prompt final e a evolução v1 → v4 estão em [`docs/prompts.md`](docs/prompts.md). Pontos principais:

- Resposta **só em JSON**, com listas fechadas de valores, para o Make conseguir ler e o Airtable aceitar.
- Mensagem delimitada por `<mensagem>…</mensagem>` e instrução para ignorar comandos dentro dela (proteção contra *prompt injection*).
- Apenas o texto da mensagem vai para a IA; nome e e-mail não.

## 7. Testes e evidências

**10 de 10 casos de teste passaram** (26/10/2026): 5 classificações corretas, 3 entradas inválidas bloqueadas antes da IA, 1 tentativa de *prompt injection* ignorada e 2 falhas forçadas desviadas para "Revisar manualmente". Detalhes, consumo de créditos e correções em [`tests/casos-de-teste.md`](tests/casos-de-teste.md). Prints em `tests/evidencias/` (e-mails reais mascarados):

| Cenário no Make | Tabela no Airtable |
| --- | --- |
| ![Cenário no Make](tests/evidencias/cenario-make.png) | ![Tabela Contatos](tests/evidencias/tabela-airtable.png) |
| **Formulário público** | **E-mail de confirmação** |
| ![Formulário](tests/evidencias/formulario.png) | ![E-mail de confirmação](tests/evidencias/email-confirmacao.png) |

**Protótipo:**

- Formulário público: https://airtable.com/appq3m6J0YvqZktsI/pagHe8xNDvYdIGMZr/form
- Cenário no Make: *Triagem de Contatos com IA* (verifica novos contatos a cada 15 minutos quando ativado)

## 8. Segurança

- Nenhuma chave de API no repositório, nos prompts ou nos prints. As conexões ficam guardadas no Make.
- A base do Airtable é privada; só o formulário é público.
- Antes de subir `config/blueprint.json`, confira que ele não contém e-mails pessoais nem tokens.

## 9. Reflexão crítica

Três vantagens e três limitações em [`docs/reflexao.md`](docs/reflexao.md).

## 10. Uso de IA no desenvolvimento

Ferramentas de IA apoiaram a definição do escopo, a escrita e revisão dos prompts, a montagem do cenário e a documentação. Toda lógica gerada por IA foi testada antes de ser aceita.

## Estrutura do repositório

```text
AutoTria/
├── LICENSE
├── README.md
├── docs/
│   ├── diagrams/fluxo.png
│   ├── prompts.md
│   └── reflexao.md
├── tests/
│   ├── casos-de-teste.md
│   └── evidencias/
└── config/
    └── blueprint.json
```
