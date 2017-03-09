---
layout: post
title: "Python Eficaz #007 -Boas Práticas com Atributos e Métodos"
description: "Discutindo sobre atributos e métodos em python."
tags: [Python]
modified: 2017-03-09

comments: false
image:
  feature: capas/python-eficaz.gif
---


## Atributos Públicos vs Privativos

Em python há dois tipo de visibilidade: públicos e privativos. porem, os atributos privativos não verdadeiramente privados. Os programadores Python seguem a risca uma convenção definida pela PEP8 onde campo prefixados com underscore (_campo) são protegidos e servem de alerta para outros programadores externos para terem cautela.

Alguns programadores novos confundem seu uso e utilizam para determinar atributos internos que não podem ser usados externamente, o que está errado. O único momento que devemos considerar usar atributos desse tipo é quando estamos preocupados com conflitos de nomes entre subclasses.

Planeje e documente bem o uso desses campos protegidos para evitar que outros programadores criem meios de forçar acesso e danificar a funcionalidade da classe.

Em resumo, prefira sempre usar atributos e métodos públicos.

:exclamation: Dica:  Evite usar “```__```“. Não precisamos usar “```__```” para dificultar ainda mais o acesso à atributos/métodos privativos. Além disso o uso do “```__```” traz alguns incoveninentes para quem quer derivar a sua classe e acessar atributos/métodos já que o Python o renomeia acrescentando o nome da classe ao seu nome (```__```attr vira ```_Classe__attr```).

## Property, Padrão Get/Set e Atributos

Programadores acostumados com Java utilizam muito as cláusulas ‘private‘ e ‘protected‘ para encapsular os atributos de um objeto para logo depois implementarem os getters e setters para acessar esses atributos.

Essa prática é aconselhada em Java porque em algum momento do futuro você, talvez, precise validar esses dados ou retornar valores calculados. Nestes casos os programadores apenas implementam essa lógica nos métodos que acessam o atributo privado.

Mas em Python isso não é necessário. Em Python você pode transformar seu atributo público em uma “property” que não muda a forma de se acessar o atributo e permite o acrescimo de lógica ao acesso do mesmo.

Outro vício vindo de linguagens como o Java é o uso de Get e Set. Isso não é considerado um código pythônico. Por exemplo, consideramos uma classe que simula um resistor:
```python
class Resistor:

  def __init__(self, ohms)
    self._ohms = ohms

  def get_ohms(self):
    return self._ohms

  def set_ohms(self, ohms):
    if ohms <=0:
        raise ValueError("Valores de ohms precisão ser maiores que zero")
    self._ohms = ohms

# Esses métodos são desajeitados para operações de atualizações
r1 = Resistor(50e3)
r1.set_ohms(r1.get_ohms() + 5e3)
```
As propriedades do Python (@property) são muito mais limpas do que getters e setter. Vamos dar uma olhada no seguinte trecho de código, escrito em um estilo Java, com getters e setters:
```python
class A:

  def __init__(self, some)
    self._b = some

  def get_b(self):
    return self._b

  def set_b(self, val):
    self._b = val

a = A('123')
print(a.get_b())
a.set_b('444')
```
Agora compare isso com um código escrito usando propriedades de estilo Python:
```python
class A:

  def __init__(self, some)
    self._b = some

  @property
  def b(self):
    return self._b

  @b.setter
  def b_setter(self, val):
    self._b = val

a = A('123')
print(a.b)
a.b = '123'

```
Nosso exemplo de Resistor ficaria, segundo esse modelo, assim:
```python
class Resistor:

  def __init__(self, ohms)
    self._ohms = ohms
   
  @property
  def ohms(self):
    return self._ohms

  @b.setter
  def ohms_setter(self, ohms):
    if ohms <=0:
        raise ValueError("Valores de ohms precisão ser maiores que zero")
    self._ohms = ohms
    

# Mais claro,mais preciso
r1 = Resistor(50e3)
r1.ohms += 5e3
```
:exclamation: __Lembre-se:__ se você encapsular lógica complicada ou cálculos pesados por trás da propriedade, você pode confundir outros desenvolvedores que usarão sua classe.

:exclamation: __Dica:__ __@property__ pode ser usada até mesmo para transformar em imutável os atributos da classe-mãe.

```python
class FixedResistance(Resistor):
  # ...

  @b.setter
  def ohms_setter(self, ohms):
    if hasattr(self,'_ohms'):
        raise AttributeError("Valores de ohms não podem ser alterados após o primeiro preenchimento")
    self._ohms = ohms
    
r1 = Resistor(50e3)
r1.ohms = 5e3
>>> 
AttributeError: Can't set attribute
```

:exclamation: __Lembre-se:__ Use __@property__ para definir qualquer comportamento especial quando os atributos são acessados e somente se for necessário. Garanta que sejam rápidos, se forem lentos ou complexos, utilize métodos normais para isso.

:exclamation: __Lembre-se:__ Você pode usar __@property__ para implementar funcionalidades novas para atributos existentes, mas sempre considere refatorar uma classes e todos os código quando perceber que está usando __@property__ demais.

## Não sobrescreva builtins.

Python disponibiliza várias funções e classes builtins que facilitam muito o uso da linguagem e economizam digitação. Mas esses builtins tem nomes muito “comuns” e frequentemente programadores usam os nomes dos builtins como nomes de identificadores. 

O problema é que em certos momentos alguns problemas podem acontecer quando você tenta chamar um buitin que já não é mais um builtin. Na maioria das vezes o problema “explode” logo e você rapidamente conserta mas em alguns casos você pode perder muitas horas tentando achá-lo.

:exclamation: Dica: você pode substituir o nome dos builtins de acordo com uma tabela de equivalências:

    id – ident, key
    type – kind, format
    object – obj
    list – plural (lista de element vira elements)
    file – fd, file_handler
    dict – dic, hashmap
    str – text, msg

## Máximo de 2 indireções.

Recebeu um objeto como parâmetro? Chame apenas métodos dele e evite ao máximo chamar métodos do retorno desses objetos:
```python
def f(obj):
    obj.metodo() # legal!
    obj.metodo().outro_metodo() # ruim!
```
Quando você chama um método pra um objeto retornado por outro método você está aumentando o acoplamento entre as classes envolvidas impedindo que uma delas seja substituída (ou reimplementada) ‘impunemente’.

