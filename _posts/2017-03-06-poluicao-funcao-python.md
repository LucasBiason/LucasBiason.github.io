---
layout: post
title: "Python Eficaz #003 - Poluição Visual em Funções"
description: "Falando sobre uso de funções de maneira eficaz em python."
tags: [python, function, funções]
modified: 2017-03-06

comments: false
image:
  feature: capas/python-eficaz.gif
---



Aqui veremos alguns tópicos sobre escrever funções melhores e mais legíveis.

## Reduza a poluição visual com argumentos opcionais

Aceitar opcionalmente parâmetros posicionais (```args```) pode tornar a chamada à função mais clara e remover uma certa quantidade de poluição visual.
```python
   def log(mensagem, valores):
       ....
   
   log("Action 1: ...", [1,2,3,4,5])
```
Se torna:
```python
   def log(mensagem,*valores):
        ....
      
   log("Action 1: ...",1,2,3,4,5)
   log("Action 1: ...", *lista) #Caso você já tenha uma lista
```
Existem duas consideraçes a serem feitas. 

1. A Primeira é que os argumentos são sempre transformados em um tupla antes de serem passado para a função (ou seja, não utilize em caso de geradores, os itens deles serão todos processados ate a exaustão do programa, podendo fazer com que esgote a memoria e trave). **Sendo assim, os __```*args```__ são mais bem empregados em situações onde conhecemos a quantidade de argumentos e esta é razoavelmente pequena.**

2. São será possível adicionar novos argumentos á função sem que todos os chamadores tenham de ser modificados. Ou seja, criar novos argumantos na função, será necessário refatorar o seu código em todos os locais que chama a função:
```python
   def log(mensagem, tipo, *valores):
        ....
   
   log("Action 1: ...", 'sucesso', 1,2,3,4,5)
```

## Implemente comportamento opcional usando palavras-chave como argumentos

Python permite configurar uma função com argumentos que podem ou não ser passados, e com isso modificar o comportamento dentro dela. (Como não implementa sobrecarga de método, essa funcionalidade substituiu isso)

Essa flexibilidade acarreta três benefícios:

1. Clareza, por exemplo:
   ```python
     criar_log("Mensagem","Alteração de cadastro",1)
   ```
   Utilizando argumentos opcionais fica mais legivel entender o que será cada um dos parâmetros:
   ```python
     criar_log(titulo="Mensagem", msg="Alteração de cadastro", tipo_log=1)
   ```

2. Os argumentos podem ter valores default, assegurando flexibilidade. Permite que uma função ofereça funcionalidades opcionais quando necessário, mas deixa que o chamador aceite os valores __default__, assim eliminando código repetitivo e ruído.

3. Substitui o conceito de sobrecarga: oferece uma maneira de estender parâmetros de funções ao mesmo que tempo em que permanece compatível com os chamadores existentes. Permitindo aumentar as funcionalidades de uma função sem alterar os códigos dos chamadores existentes. Por exemplo, imagine uma função que filtra dados em uma lista:
   ```python
     def filtrar_produtos(fornecedor):
         lista_produtos = ...gerando uma lista de todos os produtos
         lista_produtos = .... aplicando filtro de fornecedor
         return lista_produtos
             
      # Código da tela de consulta de um fornecedor
      lista = filtrar_produtos(fornecedor="Latas LTDA")
   ```
   Essa função irá produzir uma lista dos produtos cadastrados para uma tela de consulta de um fornecedor específico.
   
   Digamos que eu precise, em outra tela, apenas informar ano de fabricação dos produtos, de modo geral sem necessidade de fornecedor.
   
   Sem os argumentos em palavras chave, teriamos que alterar o código do chamador da tela de consulta de um fornecedor específico alem da nossa, ou pior, como não teríamos o comportamento do filtro de fornecedor teriamos que duplicar a função. Veja:
   ```python
     def filtrar_produtos_por_fornecedor(fornecedor):
         lista_produtos = .... aplicando filtro de fornecedor
         return lista_produtos
         
     def filtrar_produtos_por_ano(ano):
         lista_produtos = .... aplicando filtro de ano
         return lista_produtos
      
      # Código da tela de consulta de um fornecedor
      lista = filtrar_produtos_por_fornecedor("Latas LTDA")
      
      # Código da tela geral
      lista = filtrar_produtos_por_ano(2016)
   ```
   Porém, com esses argumentos, podemos adicionar os filtros necessários em uma só função
   ```python
     def filtrar_produtos(fornecedor=None, ano=None):
         lista_produtos = ...gerando uma lista de todos os produtos
         if fornecedor:
             lista_produtos = .... aplicando filtro de fornecedor
         if ano:
             lista_produtos = .... aplicando filtro de ano
         return lista_produtos
      
      # Código da tela de consulta de um fornecedor (fica intacto, sem mexer)
      lista = filtrar_produtos(fornecedor="Latas LTDA")
      
      # Código da tela Geral
      lista = filtrar_produtos(ano=2016)
   ```
   E se precisarmos adicionar um terceiro filtro? Não tem problema e não vamos precisar mexer no que já existe. Fora que poderemos mesclar as funcionalidades existentes com as novas.
   ```python
     def filtrar_produtos(fornecedor=None, ano=None, aprovados=None):
         lista_produtos = ...gerando uma lista de todos os produtos
         if fornecedor:
             lista_produtos = .... aplicando filtro de fornecedor
         if ano:
             lista_produtos = .... aplicando filtro de ano
         if not aprovados is None:
             lista_produtos = .... aplicando filtro de aprovados
         return lista_produtos
      
      # Código da tela de consulta de um fornecedor  (fica intacto, sem mexer)
      lista = filtrar_produtos(fornecedor="Latas LTDA")
      
      # Código da tela Geral  (fica intacto, sem mexer)
      lista = filtrar_produtos(ano=2016)
      
      # Código da tela completa
      lista = filtrar_produtos(fornecedor="Latas LTDA", ano=2016, aprovados=True)
   ```
   Isso é importante, pois muitas vezes não sabemos onde estão os chamadores em nosso projeto, e perdemos tempo procurando e alterando eles. Fora que teríamos que testar e validar as telas e processos dele. Muito retrabalho e perda de tempo. 
   
   Aplicando isso, não importa onde eles estão, ficarão intactos (desde que estejam os esse padrão) e só precisamos nos preocupar com a nossa tela ou processo.
   
:exclamation: Dica: Use None como valor default para os argumentos de palavra-chave que devam assumir valores dinâmicos (como por exemplo, objetos de classes que implementem uma lógica comum (polimorfismo) )

:exclamation: Dica: Use docstrings para especificar argumentos default dinâmicos e específicos, seja claro, coloque o nome e o que ele deve/pode receber.

:exclamation: Dica: Argumentos por palavra-chave tornam a intenção da função mais clara, mas cuidado, use-os em funções potencialmente confusas (aceitam muitos argumentos do mesmo tipo, principalmente booleanos) ou quando quer aplicar comportamento opcional.

##  Saiba os escopos das variáveis

Quando uma variável é referenciada em uma expressão, o interpretador ira verificar os limites de escopo na seguinte ordem:

1. O escopo da função atual
2. Quaisquer escopos que contenham o atuial (exemplo: funçes que declarem a atual)
3. O escopo do módulo que contém o código (escopo global)
4. O escopo nativo (funções como as do python: len, str, ....)

Se nenhum desses locais possuir uma variável definida com o nome, uma exceção nameError é gerada.

Vamos analisar:
```python
   def log(mensagem, tipo, *valores):
        ....
        gerou = "" # Escopo é "log"
        def gerar_log(...):
            ...
            gerou = True # Escopo é 'gerar_log' !
        gerar_log(...)
        return gerou
            
```
Em Python 3 temos uma sintaxe especial para extrair dados assim. O comando __nonlocal__ é usado para isso. O único fator limitante é que ele não ousaá cruzar o limite de escopo do módulo (no exempol acima, o escolo de "log"), evitando poí-lo com vaiáves globais mutias vezes sem necessidade.
```python
   def log(mensagem, tipo, *valores):
        ....
        gerou = "" # Escopo é "log"
        def gerar_log(...):
            nonlocal gerou # Escopo é 'log'
            ...
            gerou = True 
        gerar_log(...)
        return gerou
            
```
Esse comando deixa claro que o dado está sendo atribuído em outro escopo, fora da função (closure). É complementar o comando __global__ que força a definição no escopo global do módulo.

Em Python 2 não temos suporte ao __nonlocal__, para isso, utilize uma variável mutável (por exemplo, lista) para contornar a ausência do comando ou uma variavel com outro nome e retorne o seu valor caso necessário.

:exclamation: Dica: evite usar __nonlocal__ para funções complexas, use-o apenas em casos simples.


## Links Úteis

<div markdown="0"><a href="https://www.python.org/dev/peps/pep-3104/" class="btn btn-success">PEP 3104 -- Access to Names in Outer Scopes</a></div>
<div markdown="0"><a href="https://www.programiz.com/python-programming/function-argument" class="btn btn-success">Python Function Arguments</a></div>
