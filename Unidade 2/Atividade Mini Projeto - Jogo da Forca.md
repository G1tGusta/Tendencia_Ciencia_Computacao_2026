# Programação Assistida e Automação com IA

## Identificação
**Nome:** [Gustavo Lucas Santos Silva]
**Turma:** [N1 - Ciência da Computação - Noturno]
**Data:** 10/09/2026
**Ferramenta de IA utilizada:** Gemini

## 1. Problema
Criar um jogo da forca com alta rejogabilidade. A tarefa de selecionar palavras precisava ser automatizada para que o jogo buscasse aleatoriamente de um vasto banco de dados online, ao invés de depender de uma lista curta e estática, além de garantir que as palavras fossem formatadas (sem acentos e com tamanho máximo) de forma autônoma.

## 2. Entrada
- Dados da rede (banco de palavras em formato `.txt` via URL no GitHub).
- Letras únicas digitadas pelo usuário no console durante o jogo.

## 3. Processamento
- Requisição HTTP para baixar a lista de palavras.
- Tratamento dos dados retornados: conversão para maiúsculas, remoção de caracteres especiais/acentos (usando `unicodedata`).
- Filtro (list comprehension) garantindo que as palavras selecionadas tenham entre 5 e 12 caracteres.
- Tratamento de exceção (fallback) caso o ambiente esteja offline, acionando uma lista padrão.
- Loop principal do jogo para verificar a letra inserida e decrementar as tentativas em caso de erro.

## 4. Saída esperada
- Interface textual dinâmica no console apresentando o estado atual da palavra (com `_` para letras ocultas).
- Feedback visual sobre letras usadas, tentativas restantes e mensagens de vitória ou derrota.

## 5. Prompt utilizado
**Prompt Base:** "Atue como um dev Python Sênior. Crie a lógica de um jogo da forca modular, separando a verificação de letras em uma função específica."

## 6. Código inicial
```python
import random

def escolher_palavra():
    palavras = ["python", "refatoracao", "inteligencia", "algoritmo", "desenvolvedor"]
    return random.choice(palavras).upper()

def verificar_letra(letra, palavra, letras_descobertas):
    acertou = False
    for i, char in enumerate(palavra):
        if char == letra:
            letras_descobertas[i] = letra
            acertou = True
    return acertou

def jogar_forca():
    palavra = escolher_palavra()
    letras_descobertas = ["_" for _ in palavra]
    tentativas_restantes = 6
    letras_usadas = set()

    print("--- Bem-vindo ao Jogo da Forca Assistido por IA! ---")

    while tentativas_restantes > 0 and "_" in letras_descobertas:
        print(f"\nPalavra atual: {' '.join(letras_descobertas)}")
        print(f"Tentativas restantes: {tentativas_restantes}")
        print(f"Letras já tentadas: {', '.join(sorted(letras_usadas)) if letras_usadas else 'Nenhuma'}")

        letra = input("Digite uma letra: ").upper()

        if not letra.isalpha() or len(letra) != 1:
            print("Entrada inválida. Por favor, digite apenas UMA letra.")
            continue

        if letra in letras_usadas:
            print("Você já tentou essa letra! Escolha outra.")
            continue

        letras_usadas.add(letra)

        if verificar_letra(letra, palavra, letras_descobertas):
            print("Boa! Você acertou uma letra.")
        else:
            tentativas_restantes -= 1
            print("Que pena! A letra não está na palavra.")

    if "_" not in letras_descobertas:
        print(f"\n🎉 Parabéns! Você adivinhou a palavra: {palavra}")
    else:
        print(f"\n💀 Game Over! A palavra correta era: {palavra}")

if __name__ == "__main__":
    jogar_forca()
```

## 7. Análise crítica
O código inicial entregou o "boilerplate" (estrutura base) funcional perfeitamente. No entanto, o escopo das palavras era muito pequeno e fixo. Como "Tech Lead", notei que o código não estava abrangente, não havia tratamento para palavras com acentuação, e a falta de controle sobre o tamanho das palavras na expansão poderia gerar uma UX ruim. O tratamento de erro inicial focou apenas no input do usuário, mas precisava prever falhas externas.

## 8. Casos de teste
### Teste 1: Caso Normal
**Ação:** Usuário insere uma letra correta (ex: 'A' em "PYTHON").
**Resultado esperado e obtido:** A letra é revelada, a lista de letras usadas é atualizada, e as tentativas não são reduzidas. 

### Teste 2: Caso de Limite / Offline
**Ação:** O computador do usuário é desconectado da internet antes de rodar o script.
**Resultado esperado e obtido:** O bloco `try/except` captura a falha da URL, aciona o aviso de uso offline e seleciona uma palavra segura da lista local restrita a 12 caracteres (ex: "PROGRAMADOR"), permitindo jogar normalmente.

### Teste 3: Caso de Erro
**Ação:** Usuário digita 'AB', '1' ou deixa em branco.
**Resultado esperado e obtido:** A validação `letra.isalpha()` e `len(letra) != 1` barra a entrada, retorna o aviso "Entrada inválida" e impede que as tentativas sejam descontadas injustamente.

## 9. Problemas encontrados
Ao expandir para a lista da internet (primeira refatoração), o banco de palavras trouxe termos excessivamente longos (ex: "OFTALMOTORRINOLARINGOLOGISTA") e com acentuação, causando bugs, pois a validação do jogador seria sem acento, nunca correspondendo à palavra armazenada.

## 10. Prompt de refatoração
**Prompt 1:** "gostaria que tivesse uma forma de adicionar mais palavras ou fazer as palavras selecionadas mais abrangente pegando de algum outro lugar palavras aleatorias para ter mais abrangencia de palavras para o jogo."
**Prompt 2 (Correção de Regra de Negócio):** "as palavras precisam ter um limite de até 12 caracteres"

## 11. Código refatorado
```python
import random
import urllib.request
import unicodedata

def remover_acentos(texto):
    nfkd = unicodedata.normalize('NFKD', texto)
    return "".join([c for c in nfkd if not unicodedata.combining(c)])

def escolher_palavra():
    url = "https://raw.githubusercontent.com/pythonprobr/palavras/master/palavras.txt"
    try:
        print("Buscando banco de palavras da internet...")
        resposta = urllib.request.urlopen(url, timeout=5)
        texto = resposta.read().decode('utf-8')
        palavras = texto.split('\n')
        # Filtro pythônico: palavras entre 5 e 12 caracteres
        palavras_validas = [p.strip() for p in palavras if 4 < len(p.strip()) <= 12]
        palavra_escolhida = random.choice(palavras_validas)
        return remover_acentos(palavra_escolhida).upper()
    except Exception as e:
        print("Aviso: Não foi possível acessar a internet. Usando lista local offline.")
        palavras_padrao = ["PYTHON", "REFATORACAO", "INTELIGENCIA", "ALGORITMO", "PROGRAMADOR"]
        return random.choice(palavras_padrao).upper()

def verificar_letra(letra, palavra, letras_descobertas):
    acertou = False
    for i, char in enumerate(palavra):
        if char == letra:
            letras_descobertas[i] = letra
            acertou = True
    return acertou

def jogar_forca():
    print("--- Bem-vindo ao Jogo da Forca Assistido por IA! ---")
    palavra = escolher_palavra()
    letras_descobertas = ["_" for _ in palavra]
    tentativas_restantes = 6
    letras_usadas = set()

    while tentativas_restantes > 0 and "_" in letras_descobertas:
        print(f"\nPalavra atual: {' '.join(letras_descobertas)}")
        print(f"Tentativas restantes: {tentativas_restantes}")
        print(f"Letras já tentadas: {', '.join(sorted(letras_usadas)) if letras_usadas else 'Nenhuma'}")
        letra = input("Digite uma letra: ").upper()

        if not letra.isalpha() or len(letra) != 1:
            print("Entrada inválida. Por favor, digite apenas UMA letra.")
            continue
        if letra in letras_usadas:
            print("Você já tentou essa letra! Escolha outra.")
            continue

        letras_usadas.add(letra)

        if verificar_letra(letra, palavra, letras_descobertas):
            print("Boa! Você acertou uma letra.")
        else:
            tentativas_restantes -= 1
            print("Que pena! A letra não está na palavra.")

    if "_" not in letras_descobertas:
        print(f"\n🎉 Parabéns! Você adivinhou a palavra: {palavra}")
    else:
        print(f"\n💀 Game Over! A palavra correta era: {palavra}")

if __name__ == "__main__":
    jogar_forca()
```

## 12. Comparação
Critério | Inicial | Refatorado |
--- | --- | --- |
Funcionamento | 4 | 5 |
Clareza | 4 | 5 |
Organização | 4 | 5 |
Legibilidade | 4 | 4 |
Tratamento de erros | 2 | 5 |

## 13. Reflexão
- **Onde a IA mais ajudou?** Na montagem rápida do esqueleto (boilerplate) e na introdução ágil da biblioteca `urllib` para requisições HTTP e da função de manipulação de `unicodedata` para remover os acentos, o que poupou muito tempo de digitação.
- **Onde a IA errou?** A IA não previu sozinha o impacto das palavras acentuadas no arquivo online e a frustração do usuário ao receber uma palavra de mais de 15 caracteres do dicionário externo, pois focou apenas na sintaxe.
- **O que precisei modificar?** Como Tech Lead, direcionei a IA a criar restrições e regras de negócio claras: limite de 12 caracteres (usando slicing/list comprehension) e remoção de acentos para uniformizar a validação.
- **Consigo explicar o código?** Sim. O código está modularizado e os loops (como o bloco de exceção try/except e o iterador de filtro de lista) são amplamente legíveis e cumprem um propósito prático de resiliência.

## 14. Take Away
Programar com IA não significa **transferir toda a lógica e validação para a máquina. Significa ganhar um parceiro que digita e estrutura rápido, permitindo que eu gaste meu tempo definindo regras de negócio corretas, garantindo a UX, validando os limites (testes) e mantendo a responsabilidade técnica sobre o produto final.**
