# Trabalhando com Sequências e Conjunto de Dados

O python tem uma sintaxe específica para dividir sequências em pedaços menores. O fatiamento permite acessar um subconjunto dos itens de uma sequência com muito menos esforço. 

Evite ser prolixo: não digite 0 para start ou o comprimento da lista para end.

Ao fatiar uma lista incluindo na fatia seu inicio, você deve suprimir o índice zero para reduzir a poluição visual.
```python
a[:5] e não a[0:5]
```

Quando a fatia compreender um ponto intermediário até o final da lista ė necessário suprimir o índice final por ser redundante.
```python
a[5:] e não a[5:len(a)]
```

Evite star, end e stride em uma mesma fatia. Especificar simultaneamente os três em um slide pode ser extremamente confuso.
```python
lista[start:end:stride]
a[2::2]
a[-2:2:2]
```

Se for realmente necessário usar, prefira usar uma variável intermediária e fazer  duas atribuições ou então use o método islice do módulo itertools.

## Saiba usar abrangência de listas

O python oferece uma sintaxe compacta para derivar uma lista de outra. Essas expressões são chamadas de abrangência de lista ou list comprehensions.

Use abrangência de lista em vez de mapa e filter. Elas são mais fáceis de ler que as funções map e filter porque não precisam de expressões lambda auxiliares.
```python
Nova_lista = map( lambda X:X , filter( lambda X: X >0 ,lista) )
```
Ficaria:
```python
Nova_lista = [ X for X in lista if X >0]
```

Evite mais de duas expressões em abrangência se lista. Elas são muito difíceis de ler e entender e devem ser evitadas ao máximo.

**Dica:** Considere usar expressões geradoras em abrangências muito grandes.
As abrangências podem causar problemas quando o fluxo de dados na entrada for muito intenso, consumindo muita memória. As geradoras evitam os problemas porque devolvem um iterador que produz apenas um valor por vez.
```python
Nova_lista = ( X for X in open('arquivo.csv') )
```
Quando aninhadas juntas, duas ou mais expressões geradoras executam de forma veloz no python.

**Dica:** prefira enumerate em vez de range.
Ele oferece uma sintaxe concisa para laços em iteradores e para obter índice de cada item do iterador durante o andamento da varredura. 
```python
for i, dado in enumerate(lista, 1):
  ....
```

**Dica:** use zíper para processar iteradores em paralelo. 
```python
for nome, telefone in zip(nomes, telefones):
  ....
```

**Nota:** Em __2.i Escreva Funções auxiliares em vez de expressões complexas__ veremos outra dica sobre o uso de abrangências e funções como map, filter e reduce para manter o código limpo e legível.


## Aprenda a Usar Dicionários

Dicionários são estruturas poderosas e fáceis de utilizar em Python. São encontradas em outras linguagens como tipos de "Arrays associativos". Em Python, é um conjunto (**não preserva ordem**) de chaves (únicas) e valores.
Temos o seguinte exemplo:
```python
if name == "John":
   print "This is John, he is an artist"
elif name == "Ted":
   print "This is Ted, he is an engineer"
elif name == "Kennedy":
   print "This is Kennedy, he is a teacher"
```
Utilizando Dicionario:
```python
name_job_dict = {
   "Josh": "This is John, he is an artist",
   "Ted": "This is Ted, he is an engineer",   
   "Kenedy": "This is Kennedy, he is a teacher",
}
print name_job_dict[name]
```
Podemos utilizar essa técnica para substituir muitos if, um dicionário pode ter Classes, Funções, Dados e até outros dicionário. Utilize um dicionário de funções para substituir if/elifs quando necessário determinar o valor de algum case para executar uma determinada função.

## Dicas sobre coleções em Python:

1. Verificar se uma lista está vazia

 Não é necessário chamar a função len para verificar se uma lista está vazia porque uma lista vazia é avaliada como False. Então, em vez de fazer:
 ```python
if len(mylist):
    # ....
else:
    # A lista está vazia...
 ```
 Você pode simplesmente fazer
 ```python
if mylist:
    # ....
else:
    # A lista está vazia...
```

2.  Obtendo índices de elementos enquanto itera sobre uma lista

 Às vezes você precisa iterar sobre uma lista e ao mesmo tempo obter o índice de cada elemento. Em vez de fazer:
 ```python
i = 0
for element in mylist:
    # ....
    i += 1
 ```
 Você pode simplesmente fazer
 ```python
for i, element in enumerate(mylist):
    # ....
 ```

3. Classificar uma lista

 É bastante comum classificar uma lista com base em uma das características de seus elementos. Aqui, por exemplo, criamos uma lista de pessoas:
 ```python
 class Person(object):
    def __init__(self, age):
        self.age = age

 persons = [Person(age) for age in (14, 78, 42)]
 ```
 Então, queremos ordenar a lista com base na idade. Aqui está como nós poderia fazê-lo:
 ```python
 def get_sort_key(element):
    return element.age

 for element in sorted(persons, key=get_sort_key):
    print "Idade:", element.age
 ```
 Definimos uma função que retorna o atributo que queremos usar como critério de classificação e passamos essa função como argumento chave para a função classificada. Esse tipo de classificação é tão comum que a biblioteca padrão do Python inclui funções prontas para fazer isso.
 ```python
 from operator import attrgetter

 for element in sorted(persons, key=attrgetter('age')):
    print "Idade:", element.age
 ```
 Attrgetter é uma função de ordem superior que retorna uma função semelhante à get_sort_key. Salvamos algumas batidas de tecla,mas mais importante, o código é fácil de ler. Quando você vê attrgetter você sabe imediatamente o que ele vai receber um atributo. O módulo de operador também fornece itemgetter e methodcaller e tenho certeza que você pode adivinhar o que eles fazem.

4. Agrupando elementos em um dicionário

 A última dica que vou lhe dar hoje é sobre dicionários. É uma tarefa bastante comum agrupar elementos de uma lista com base em um critério. Para isso construímos um dicionário de listas indexadas por esse critério. Digamos que temos uma lista de pessoas.
 ```python
 class Person(object):
    def __init__(self, age):
        self.age = age

 persons = [Person(age) for age in (78, 14, 78, 42, 14)]
 ```
 Agora queremos agrupar essas pessoas por idade. Uma abordagem poderia ser:
  ```python
 persons_by_age = {}

 for person in persons:
    age = person.age
    if age in persons_by_age:
        persons_by_age[age].append(person)
    else:
        persons_by_age[age] = [person]

 assert len(persons_by_age[78]) == 2
  ```
  Para cada iteração testamos se a chave existe usando o operador in. Se ele não estiver presente, precisamos criar uma lista para essa chave usando o elemento atual.

 O módulo de coleções oferece um padrão que simplifica esse processo.
 ```python
 from collections import defaultdict

 persons_by_age = defaultdict(list)

 for person in persons:
    persons_by_age[person.age].append(person)
  ```
  Quando você cria um defaultdict, você passa um callable que ele usará para criar valores para o dict quando uma chave está faltando. Aqui vamos passá-lo lista para que ele vai criar uma lista para cada nova chave.
  
###### Links úteis:

:clipboard: [Itertools em Python 2](https://docs.python.org/2/library/itertools.html)

:clipboard: [Itertools em Python 3](https://docs.python.org/3/library/itertools.html)

:clipboard: [list comprehensions em Python 2](https://docs.python.org/2/tutorial/datastructures.html#list-comprehensions)

:clipboard: [list comprehensions em Python 3](https://docs.python.org/3.5/tutorial/datastructures.html#list-comprehensions)


============================
:arrow_left: [Voltar](https://github.com/LucasBiason/PadroesPython/blob/master/python_eficaz/boas_praticas.md)
