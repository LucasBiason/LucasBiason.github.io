---
layout: post
title: "Python Eficaz #002 - Funções Auxiliares vs. Expressões Complexas"
description: "Falando sobre uso de funções de maneira eficaz em python."
tags: [python, function, funções, lambda]
modified: 2017-03-05

comments: false
image:
  feature: svg-animate-along-path-600.gif
---

Vamos falar aqui sobre funções em python, normais ou lambda, uma das dúvidas de quem esta começando é e qual tipo usar, onde usar e pra usar. Para programar de maneira eficiente algumas consideraçoes devem ser feitas...

No lugar de escrever expressões repeditas e de dificil visualização, crie uma função para organizar, melhor a legibilidade e reaproveitar o seu código.

Exemplo: digamos que seja necessário decoficar parâmetros de uma url
```python
from urllib.parse import parse_qs
my_values = parse_qs('red=5&blue=0&green=', keep_blank_values=True)
red = my_values.get('red', [''])[0] or 0
green = my_values.get('green', [''])[0] or 0
opacity = my_values.get('blue', [''])[0] or 0
```
Essa expressão pode ser transferida para uma função simples:
```python
def get_first_int(values, key, default=0):
    found values.get(key, [''])
    if found[0]:
        retorn int(found[0])
    retorn default
```
Assim, nosso código ficaria:
```python
red = get_first_int(my_values,'red')  
green = get_first_int(my_values,'green') 
opacity = get_first_int(my_values,'blue')
```

A sintaxe do python facilita a criação de expressões de uma única linha que são incrivelmente complexas e difíceis de ler.

Transfira expressões complexas para uma função auxiliar. Especialmente se for necessário usar a mesma lógica repetidamente.

A expressão if/else oferece uma alternativa muito mais legível que o emprego de operadores booleano se como or e and em expressões.

**Case:** muitas vezes criamos expressões complexas usando abrangências de listas e lambdas e outros. Por exemplo, imagine que eu receba parametros diversos com as seguintes chaves: "dados[xxx][id]", sendo xxx um id. Eu preciso pegar esses Ids (extraindo o número da chave) e gerar indices apenas dos válidos (verificando com regexp), ficaria a principio assim:
```python
    indices = [k.replace("dados[", "").replace("][id]", "") for k in filter(lambda k: re.match(r'^dados\[(\d+)\]\[id\]$', k), dados.keys())]
```
* **Nota:** código tirado de um exemplo real

Um código de dificil manutenção e entendimento, fora que fere os princípios da PEP8. Aplicando o conhecimento ate agora, podemos separar o código em rechos, facilitando a leitura (mesmo que o uso dele seja complexo). podemos fazer isso com lambdas ou funções. Nosso ccódigo ficaria assim:
```python
    extrair = lambda k: k.replace("dados[", "").replace("][id]", "")
    validar = lambda k: re.match(r'^dados\[(\d+)\]\[id\]$', k)
    indices = [ extrair(k) for k in dados.keys() if validar(k) ]
```
Assim fica um pouco melhor de visualizar. Ainda podemos melhorar mais, mas o intuito aqui é mostrar o quanto criar funções auxiliares nos ajuda. Um nota, outros pontos do código usam a mesma ideia da _aux_reg_, ou seja, podemos reaproveitar essa validação nos outros trechos.

<style>

</style>
<div style="display:inline-flex">
<div class="img_thumb">
   <img  class="images_class" src="https://LucasBiason.github.io/images/1488773766_caution.svg" width='50px'>
 </div>
 <div style="">
Quando você começar a dar Ctrl+c e Ctrl+v em trechos de códigos, analise se esses trechos não podem virar funções. Muitas vezes copiamos os mesmos trechos de código propagando logicas que podem estar centralizadas. Lembre-se, código repetido leva um trabalho manual e tempo para ser alterado quando precisa.
 </div>
</div>


## Lambda versus Função:

Lambda não é nada mais que funções anônimas que aceitam argumentos (inclusive opcionais) e que __só suportam uma expressão__. Ao executar lambda, Python retorna uma função ao invés de atribuí-la à um nome como acontece com def, por isso são anônimas. O conceito e o nome são emprestados de Lisp, uma linguagem funcional. 

lambda é basicamente uma questão de estilo. Elas podem ajudar seu código em termos de legibilidade e praticidade. Sempre use lambdas ao invés de várias funções de uma linha só.

Ao criar uma função lambda, certifique-se de ela não será complexa. Se o que vc escreve nela puder ser feito em mais de uma linha, utilize função no lugar. **Expressões lambdas devem ser simples.** Não se orgulhe de expressões dificeis, deixe isso com matemáticos, físicos e químicos.

No exemplo anterior, quebramos uma super expressão lambda em duas. O conceito de lambda é que ela seja simples, no nosso código podemos utilizar uma função simples no lugar dessas duas lambdas e abrangência:
```python
    ## Antes
    extrair = lambda k: k.replace("dados[", "").replace("][id]", "")
    validar = lambda k: re.match(r'^dados\[(\d+)\]\[id\]$', k)
    indices = [ extrair(k) for k in dados.keys() if validar(k) ]
    
    ## Depois
    def gerar_indices(dados):
        indices = []
        for key in dados.keys():
            if re.match(r'^dados\[(\d+)\]\[id\]$', key):
                indice = k.replace("dados[", "").replace("][id]", "")
                indices.append(indice)
        return indices
```
Conhecer os módulos do python é muito importante, com o módulo __re__ utilizando __search__ podemos melhorar o nosso código. No lugar de dois replaces, podemos procurar o fragmento que queremos dentro da string usando o __search__, se encontrar o fragmento, já adiciona na lista, senão encontrar significa que o padrão da string esta incoreto, aqui podemos utilizar o __match__ para validar ou não, dependendo do seu processo. Todas as chaves que o processo recebe são no padrão "dados[XX][id]", para assegurar, utilizamos o __match__. Veja:
```python
    def gerar_indices(dados):
        indices = []
        for key in dados.keys():
            if re.match(r'^dados\[(\d+)\]\[id\]$', key):
                resultado = re.search(r'(\d+)', key).group()
                indices.append(resultado)
        return indices
```
Substituir os dois replaces teve um ganho de performance.

## Voltando com... Aprenda a usar Dicionários:

Como dito na ultima seção de dicas, podemos utilizar dicionários para substituir cadeias de If/Elifs. No código a seguir temos a seguinte funcionalidade. Precisamos consultar a porcentagem de pedidos de uma loja virtual, temos o total deles (a variável total) e um dicionário com a quantidade de pedidos por status (o dicionário status_qtdes). Imagine que precisamos criar um gráfico para um dashboard, para isso vamos usar um dicionário com as chaves e porcentagens:
```python
status_qtdes = {
 'cancelado': 456, #Cancelado
 'aprovacao': 112, #Para Aprovação (não foi pago ainda)
 'encerrado': 5674, #Encerrado (pago e entregue)
 'andamento': 2345 #Andamento (pago e sendo entregue)
}
total_status = 0
for key, value in status_qtdes:
    total_status += value
```
Para calcular a soma dos status, poderiamos utilizar reduce:
```python
total_status = reduce(lambda x,y: x+y, status_qtdes.values()) # Simples e direto
```
Como os princípios do pythoon pedem para não reinventarmos a roda, temos a função __sum__:
```python
total_status = sum(status_qtdes.values()) # Mais Simples e Mais direto
```

Poderiamos criar esse dicionário da seguinte forma:
```python
status = {
   'cancelado': float( float(status_qtdes['cancelado'])*100/total_status) if total_status else 0, 
   'aprovacao': float( float(status_qtdes['aprovacao'])*100/total_status) if total_status else 0, 
   'encerrado': float( float(status_qtdes['encerrado'])*100/total_status) if total_status else 0, 
   'andamento': float( float(status_qtdes['andamento'])*100/total_status) if total_status else 0  
}
```
Mas, veja a quantidade de código, repetido e massivo. Para melhorar, poderiamos criar uma função mais estruturada e simples de ler que faria as contas de porcentagem e validação dos dados:
```python
def calc_total(key):
    if not total_status:
        return 0
    status_val = float(status_qtdes[key])
    return float(status_val*100/total_status)
```
O Dicionário ficaria assim:
```python
status = {
   'cancelado': calc_total('cancelado')
    'aprovacao': calc_total('aprovacao')
    'encerrado': calc_total('encerrado')
    'andamento': calc_total('andamento')
}
```
Poderiamos melhorar um pouco e escrever de outra maneira, aproveitando que, as chaves de ambos os dicionários podem ser iguais, então porque não aproveitar? ficaria assim:
```python
status = { key : calc_total(key) for key in status_qtdes.keys() }
```
**.keys()** retorna uma lista das chaves de um dicionário e **.values()** retorna uma lista dos valores dos dicionários. Sempre saiba manipular bem suas estruturas de dados conhecendo o que a linguagem pode oferecer.
Nosso Código final:
```python

status_qtdes = {
 'cancelado': 456, #Cancelado
 'aprovacao': 112, #Para Aprovação (não foi pago ainda)
 'encerrado': 5674, #Encerrado (pago e entregue)
 'andamento': 2345 #Andamento (pago e sendo entregue)
}

total_status = sum(status_qtdes.values()) # Simples e direto

def calc_total(key):
    if not total_status:
        return 0
    status_val = float(status_qtdes[key])
    return float(status_val*100/total_status)
    
status = { key : calc_total(key) for key in status_qtdes.keys() }

```
Tem um ou duas linhas a mais, porém, mais limpo, fácil de entender e dar manutenção. Caso precise acrescentar ou retirar algum status, basta alterar o status_qtdes, incluindo ou removendo.

Nosso resultado fica:
```python
>>> status
{
    'cancelado': 5.310352858972866, 
    'andamento': 27.308722487481077, 
    'encerrado': 66.07662746011412, 
    'aprovacao': 1.304297193431932
}
```

Por que não usar lambda?
```python
    calc_total = lambda key: float(float(status_qtdes[key])*100/total_status) if not total_status 0
 ```
 Dá pra imaginar que quebra nossa princípio de simplicidade e legibilidade.
   
## Links Úteis

<div markdown="0"><a href="https://docs.python.org/2/library/re.html" class="btn btn-success">RE em Python 2</a></div>
<div markdown="0"><a href="https://docs.python.org/3.5/library/re.html" class="btn btn-success">RE em Python 3</a></div>

