# Atividade - Refinamento de Prompts

## Identificação
- **Nome:** Gustavo Lucas Santos Silva
- **Turma:** Tendências em Ciência da Computação
- **Data:** 03/09/2026
- **Ferramenta de IA utilizada:** Gemini

---

## Problema escolhido

### Contexto
Estudantes iniciantes de Ciência da Computação que estão cursando a disciplina de Programação Orientada a Objetos (POO) pela primeira vez.

### Problema
Dificuldade em compreender a diferença teórica e prática entre os conceitos de **Classe** e **Objeto**, que frequentemente são confundidos pelos alunos.

### Objetivo
Obter da IA uma explicação didática, clara e com exemplos do cotidiano e de código que diferencie perfeitamente uma Classe de um Objeto para um iniciante.

---

## Prompt 1

### Prompt
> Explique a diferença entre classe e objeto.

### Resultado
> Uma classe é um modelo ou template para criar objetos. Ela define as propriedades (atributos) e os comportamentos (métodos) que os objetos criados a partir dela terão.
> Um objeto é uma instância de uma classe. É a materialização desse modelo na memória durante a execução do programa, contendo valores reais para os atributos definidos na classe.

### Análise
- **O que funcionou:** A definição técnica está correta.
- **O que faltou:** Faltaram exemplos práticos.
- **O que ficou genérico:** A explicação foi muito teórica.
- **O que poderia ser melhor:** Utilizar uma linguagem menos técnica para um iniciante, pois termos como "instância" e "materialização na memória" podem confundir.

---

## Prompt 2

### Alterações realizadas
- **Papel:** Professor de programação.
- **Contexto:** Alunos iniciantes aprendendo POO[cite: 2].
- **Objetivo:** Explicar a diferença de forma intuitiva[cite: 2].
- **Público:** Iniciantes[cite: 2].
- **Formato:** Texto curto com analogia[cite: 2].
- **Restrições:** Usar uma analogia do mundo real e limitar a 150 palavras[cite: 2].

### Prompt
> Atue como um professor de programação. Explique a diferença entre Classe e Objeto para um estudante iniciante em POO. Use uma analogia do mundo real para facilitar o entendimento. Limite a resposta a, no máximo, 150 palavras.

### Resultado
> Olá! Vamos entender Classe e Objeto de um jeito bem simples. 
> Imagine que você vai construir uma casa. A **Classe** é a planta arquitetônica dessa casa. Ela diz onde ficam as portas, janelas e quantos quartos existem. Ela é apenas o projeto, você não pode morar nela.
> O **Objeto** é a casa construída de fato a partir dessa planta. Você pode construir várias casas (Objetos) usando a mesma planta (Classe). Cada casa pode ter uma cor de parede diferente, mas todas seguem o mesmo modelo.
> Na programação é igual: a Classe define as regras e o Objeto é a criação real que usamos no código!

---

## Comparação

| Critério | Prompt 1 | Prompt 2 |
|---|---:|---:|
| Clareza | 3 | 5 |
| Precisão | 4 | 4 |
| Relevância | 3 | 5 |
| Organização | 2 | 4 |
| Adequação ao público | 2 | 5 |
| Atendimento ao objetivo | 3 | 4 |

---

## Prompt 3

### O que ainda precisava melhorar?
A analogia ficou excelente, mas faltou a ponte para a prática computacional. O aluno entende a teoria, mas ainda não sabe como isso se parece em código, nem tem uma visão resumida dos conceitos[cite: 2].

### Hipótese
Acredito que a resposta ficará melhor porque, ao adicionar um bloco de código super simples e uma tabela resumo, uniremos o entendimento conceitual (analogia) com a aplicação prática (código) e a revisão (tabela)[cite: 2].

### Prompt
> Atue como um professor universitário especialista em Programação Orientada a Objetos.
> CONTEXTO: Alunos do 2º semestre aprendendo POO em Python. Eles precisam entender a diferença entre Classe e Objeto.
> TAREFA: 
> 1. Explique a diferença usando a analogia da planta da casa.
> 2. Mostre um exemplo em código Python super simples demonstrando a classe e a criação de dois objetos.
> 3. Explique brevemente o termo técnico "instância".
> FORMATO: Use subtítulos e finalize com uma tabela comparativa direta entre Classe e Objeto.

### Resultado
> Compreender a diferença entre Classe e Objeto é o primeiro passo para dominar a Programação Orientada a Objetos.
> 
> ### A Analogia da Casa
> A **Classe** é como a planta arquitetônica de uma casa. Ela define a estrutura (quartos, portas, janelas), mas é apenas um papel; não tem existência física real. O **Objeto** é a casa construída a partir dessa planta. Você pode usar uma mesma planta (Classe) para construir diversas casas (Objetos) com características específicas (uma azul, outra amarela).
> 
> ### O Conceito de Instância
> Em POO, chamamos o ato de construir essa casa de "instanciação". Portanto, uma **instância** é simplesmente um Objeto que foi criado a partir de uma Classe.
> 
> ### Exemplo em Python
> ```python
> # A Classe (A Planta)
> class Casa:
>     def __init__(self, cor):
>         self.cor = cor
> 
> # Os Objetos (As instâncias / Casas reais)
> casa_do_joao = Casa("Azul")
> casa_da_maria = Casa("Amarela")
> ```
> 
> ### Tabela Comparativa
> | Critério | Classe | Objeto |
> |---|---|---|
> | **Natureza** | Conceitual / Molde | Concreto / Realidade |
> | **Uso de Memória** | Não ocupa espaço de dados | Ocupa espaço na memória |
> | **Declaração** | Definida uma única vez | Criado múltiplas vezes |

---

## Comparação final

| Critério | Prompt 1 | Prompt 2 | Prompt 3 |
|---|---:|---:|---:|
| Clareza | 3 | 5 | 5 |
| Precisão | 4 | 4 | 5 |
| Relevância | 3 | 5 | 5 |
| Organização | 2 | 4 | 5 |
| Adequação ao público | 2 | 5 | 5 |
| Atendimento ao objetivo | 3 | 4 | 5 |
| Utilidade | 2 | 3 | 5 |
| **Total** | **19** | **30** | **35** |

---

## Validação

Como você verificou a qualidade e correção da resposta?[cite: 2]
> Verifiquei a veracidade da informação consultando a literatura clássica de programação orientada a objetos[cite: 2]. Confirmei que o código Python gerado possuía sintaxe correta e funcionaria se executado, e validei que o termo "instância" foi conceituado da forma correta. Além disso, li a explicação para garantir que eu mesmo conseguiria explicá-la com minhas próprias palavras[cite: 2].

---

## Reflexão

### 1. Qual foi a principal diferença entre os prompts?
A transição de uma instrução genérica ("Explique a diferença") para uma instrução altamente detalhada que especifica o emissor, o receptor, o contexto de ensino e o formato exato da saída[cite: 2].

### 2. Quais elementos tiveram maior impacto?
Definir o público ("estudante iniciante") e formatar a saída (solicitar analogia, tabela e código)[cite: 2].

### 3. Um prompt maior é necessariamente melhor?
Não. Um prompt longo pode ser apenas confuso[cite: 2]. O prompt 3 não é massivo em quantidade de palavras, mas é preciso e estruturado, o que o torna eficiente[cite: 2]. Quantidade de palavras não é sinônimo de qualidade[cite: 2].

### 4. O que ocorre quando o objetivo não é claro?
A IA precisa "adivinhar" o que queremos, resultando em respostas médias e generalistas, que geralmente não se aprofundam na dor real do usuário[cite: 2].

### 5. Quais informações são indispensáveis?
O papel, o objetivo específico, o contexto do público e as restrições e formatação desejada[cite: 2].

### 6. Como essa habilidade pode ser utilizada profissionalmente?
Um profissional de Ciência da Computação pode utilizar o refinamento para gerar casos de testes complexos, explicar códigos legados, gerar documentações padronizadas e revisar arquiteturas de software de maneira muito mais eficiente e precisa[cite: 2].

### 7. Quais riscos existem ao confiar automaticamente na IA?
Obter códigos com vulnerabilidades, dados inventados (alucinações), generalizações incorretas ou adotar soluções que não se aplicam aos requisitos técnicos de um projeto real sem a devida checagem[cite: 2].

---

## Desafio final

Imagine que um colega diga:

> "Meu prompt não funcionou. A IA respondeu errado."

Antes de modificar o prompt, faça cinco perguntas para diagnosticar o problema.

1. **Qual era o seu objetivo exato e o que exatamente você escreveu no prompt?** (Para verificar se a intenção do usuário foi claramente comunicada à IA).
2. **Você definiu um papel, contexto, público-alvo ou formato de saída?** (Para checar se o prompt estava genérico ou se possuía os parâmetros necessários para guiar a resposta).
3. **A informação gerada pela IA é tecnicamente falsa (alucinação) ou apenas superficial e fora do formato que você esperava?** (Para diferenciar um erro da base de dados de um erro de formatação).
4. **O problema exigia conhecimentos muito específicos, dados em tempo real ou informações privadas que a IA não tem acesso?** (Para avaliar as limitações do modelo).
5. **Como você validou o resultado para concluir que ele está "errado"?** (Para entender o critério de avaliação e a expectativa do usuário).

### Pergunta final

> **O problema está no prompt, na resposta da IA, nas informações utilizadas ou na forma como o usuário avaliou o resultado?**

O problema pode estar em **todas essas possibilidades**. Como aponta o material, a Engenharia de Prompt não é apenas sobre escrever comandos, mas sobre investigar o ciclo completo:
- Pode ser um **prompt** ambíguo ou incompleto.
- Pode ser uma **resposta da IA** baseada em viés ou alucinação.
- Pode ser uma falha nas **informações utilizadas** (dados de treinamento desatualizados).
- Pode ser a **avaliação do usuário**, que possuía uma expectativa irreal ou não soube interpretar a saída do modelo. 
O engenheiro de prompt deve analisar cada uma dessas variáveis antes de simplesmente tentar a mesma instrução novamente.


## Take Away

> Um bom prompt não é simplesmente um prompt longo. Ele precisa **identificar quais informações são necessárias para diminuir ambiguidades e orientar a IA para o objetivo desejado, considerando perfil, tarefa, cenário e formato de saída**[cite: 2].

---

## Cinco recomendações

1. Atribua um papel e perfil especializado à IA antes de fazer o pedido.
2. Forneça o contexto do problema, explicando quem consumirá a resposta.
3. Especifique a estrutura e formato desejados (ex: tabelas, tópicos, código).
4. Inclua restrições claras (o que a IA NÃO deve fazer).
5. Sempre duvide, valide criticamente os dados e teste os códigos gerados.
