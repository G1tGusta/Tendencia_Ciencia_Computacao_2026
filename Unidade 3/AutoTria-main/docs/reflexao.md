# Reflexão crítica

## Vantagens

1. **Desenvolvimento rápido.** O fluxo completo foi montado com módulos prontos, sem escrever código de integração com Airtable, IA ou Gmail.
2. **IA resolve o que regras fixas não resolvem.** Classificar texto livre exigiria dezenas de regras por palavra-chave. Um prompt bem delimitado fez isso com saída padronizada em JSON.
3. **Custo zero e manutenção simples.** Tudo roda em planos gratuitos, e mudar uma categoria exige só editar o prompt e a opção no Airtable.

## Limitações

1. **Limites do plano gratuito.** O Make gratuito tem 1.000 créditos por mês, 2 cenários ativos e intervalo mínimo de 15 minutos entre verificações. Nos testes, cada mensagem válida custou de 5 a 6 créditos (a IA cobra por tokens), o que dá cerca de 160 mensagens por mês. O Airtable gratuito guarda 1.000 registros por base.
2. **A saída da IA não é garantida.** A IA pode classificar errado ou devolver um JSON inválido. Por isso foram necessários a rota de erro e o status "Revisar manualmente".
3. **Dependência da plataforma.** O cenário só roda no Make. Migrar para outra ferramenta significa reconstruir o fluxo (lock-in). O blueprint exportado documenta a lógica, mas não é portável.

## Uso de IA no desenvolvimento

Ferramentas de IA apoiaram o planejamento do escopo, a escrita e a revisão dos prompts, a montagem do cenário e a documentação. Toda lógica gerada foi testada com os casos em `tests/casos-de-teste.md` antes de ser aceita.

## Próximos passos

- Painel com contagem de mensagens por categoria e prioridade.
- Alerta imediato ao responsável para mensagens de prioridade alta.
- Sugestão de resposta pela IA, enviada só após aprovação humana.
