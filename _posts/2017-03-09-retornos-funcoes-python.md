---
layout: post
title: "Python Eficaz #004 - Boas Práticas com retornos em Funções"
description: "Falando sobre o que deve ou não ser retornado em funções do python."
tags: [Python]
modified: 2017-03-08

comments: false
image:
  feature: capas/python-eficaz.gif
---

Hoje termos alguns tópicos sobre o que deve ou não ser retornado em funções. Muitas vezes nos deparados com funções que retornam None, mas é por que deu erro ou no achou algo? ou o que achou era None? e listas vazias... realmente esta vazio?

## Prefira exceções em vez de devolver None

Ao criar uma função utilitária, existe um acordo entre programadores para reservar o None quando usado como valor de retorno. Em alguns casos, faz sentido, como uma função matemática ou de busca.
Usar None omovalor de retorno em uma função tende a atrair problemas.
Existe duas possíbilidade de contornar isso:

1. Utilizar dois retorno, um boolean de controle para indicar sucesso e um valor:
```python
    try:
        return True, action()
    except Exception:
        return False, None
```
Desvantagem: o primeiro argumento pode ser facilmente ignorado

2. Nunca retorne None, utilize Exception. O código que invoca a função, deve ser obrigado a tratar o problema e sem necessidade de um código condinal para isso.
```python 
    try:
        return action()
    except Exception as e:
        raise Exception("Ocorreu um erro...") from e
```
:exclamation: Funções que retornam None para indicar significados especiais estão fadadas a erros porque o None, em conjunto com outros valores são interpretados como False em expresses condicionais.

:exclamation: Gere exceções para indicar situação especial e assuma que o código invocador conseguirá tratar corretamente (se você documentar de forma apropriada)

## Prefira geradores em vez de retornar listas

Uma escolha natural para uma função que produz uma sequência seria o retorno da lista, por exemplo:
```python 
    def gerar_indices(nomes):
        resultado = []
        if nomes:
            resultado.append(0)
        for index, nome in enumerate(nomes):
            resultado.append( index + 1 )
``` 
Dois problemas são identificados: __Sujeira de código__ e existe uma maneira muito melhor de implementar isso, empregando os geradores. Empregar geradores pode tornar seu código mais legível e claro.
```python 
    def gerar_indices(nomes):
        if nomes:
            yield 0
        for index, nome in enumerate(nomes):
            yield index + 1
``` 
Um código muito mais fácil de ler, pode ser convertido em lista. Porém... ainda tem um problema. Ele precisa ter todos os resultado para poder criar a lista e isso pode gerar lentidão e travamento em grandes quantidade de dados. Por isso, podemos trocar para o seguinte:
```python 
    def gerar_indices(nomes):
        offset = -1
        for nome in nomes:
            offset += 1
            yield offset
``` 
:exclamation: Os chamadores devem estar cientes que os iteradores devolvidos pelo gerador não podem ser reutilizados.

Empregar geradores pode tornar o código mais legível e claro, além de mais eficiente.

:exclamation: Dica:

Quando uma função recebe muitos parâmetros e precisamos iterar sobre eles.

No Python, quando temos um comando do tipo __for x in foo__, o que ele faŕa é chamar __iter(foo)__, uma função nativa que invoca __iter__ do objeto que esta sendo iterado e ao passar de um para o outro, a função __next__. Se implementarmos um __iter__ em nosso contêiner, ele será iterável. Tome cuidado ao iterar sobre objetos não iteraveis, como por exemplo uma classe simples ou uma string.

Tome muito cuidado com funções que fiquem iterando mais de uma vez sobre os argumentos, tente realizar uma iteração apenas ou somente o necessário. 

## “Nada” é diferente de “alguma coisa”

Quando o seu método/função retorna uma collection (seqüência, conjunto, hash, etc) vazia você deve retorná-la vazia (caso não tenha itens para ela) e não um valor sentinela (como None). Isso facilita a vida de quem vai usar o seu método/função.


## Links Úteis

<div markdown="0"><a href="https://docs.python.org/3/tutorial/errors.html" class="btn btn-success">Errors and Exceptions em Python 3</a></div>
<div markdown="0"><a href="http://anandology.com/python-practice-book/iterators.html" class="btn btn-success">Dicas sobre iteradores</a></div>
<div markdown="0"><a href="https://www.programiz.com/python-programming/iterator" class="btn btn-success">Dicas sobre iteradores - Parte 2</a></div>

