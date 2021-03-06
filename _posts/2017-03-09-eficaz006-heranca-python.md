---
layout: post
title: "Python Eficaz #006 - Boas Práticas em Herança"
description: "O Python suporta um vasto leque em orientação a objetos, e uma funcionalidade polêmica é a herança múltipla. Falaremos aqui sobre dicas interessantes em herança e herança múltipla, quando, como e para o que utilizar. Assim como pequenas dicas relacionadas."
tags: [Python]
modified: 2017-03-09
comments: true
image:
  feature: capas/python-eficaz.gif
---

## Introdução

O Python suporta um vasto leque em orientação a objetos, e uma funcionalidade polêmica é a herança múltipla. Falaremos aqui sobre dicas interessantes em herança e herança múltipla, quando, como e para o que utilizar. Assim como pequenas dicas relacionadas.

## Inicialize classes ancestrais com super

Utilize sempre a função nativa super para inicializar classes ancestrais. Isso faz com que o algoritmo de interpretação do python faça seu trabalho de maneira correta e assegura a continuidade do processo do método na classe mãe.

Em Python 2:
```python
class C(BaseClass):
    def __init__(self, arg1, arg2, ...):
        super(C, self).__init__(arg1, arg2, ...)
        
```

Em Python 3: a versão mais recente do python, eliminou a necessidade (e incômodo) de ficar passando a classe como parâmetro do super, isso porque, ao contrário do Python 2, o __class__ já esta implementado nas classes por padrão.
```python
class C(BaseClass):
    def __init__(self, arg1, arg2, ...):
        super().__init__(arg1, arg2, ...)
        
```

## @classmethod para construir objetos genericamente

No Python, tanto objetos como classes podem sofrer mutações. O Polimorfismo de __@classmethod__ funciona da mesma maneira que o polimorfismo de método de instãncia, mas com vantagem de serem válidos para toda uma classe em vez de a cada um dos objetos individuais dela. 

O @classmethod recebe como primeiro parâmetro a classe que ele foi chamado. Métodos de classe são métodos que não são vinculados a um objeto, mas a uma classe! 
```python
class C(object):
    @classmethod
    def f(cls, arg1, arg2, ...):
        ...
```

Seja qual for a maneira que você usa para acessar este método, ele será sempre vinculado à classe que está anexada, e seu primeiro argumento será a própria classe (lembre-se que as classes são objetos também).

Use-o para implementar uma maneira genérica de construir e conectar subclasses concretas.

Outros usos:
- Métodos de fábrica, que são usados para criar uma instância para uma classe usando, por exemplo, algum tipo de pré-processamento. Se usarmos um @staticmethod em vez disso, teríamos de codificar o nome Pizza em nossa função, tornando qualquer classe herdada da Pizza incapaz de usar nossa fábrica para seu próprio uso. Exemplo:
```python
class Pizza(object):
    def __init__(self, ingredients):
        self.ingredients = ingredients
 
    @classmethod
    def from_fridge(cls, fridge):
        return cls(fridge.get_cheese() + fridge.get_vegetables())
```
- Métodos estáticos chamando métodos estáticos: se você dividir um métodos estáticos em vários métodos estáticos, não deve codificar o nome da classe mas usar métodos de classe. Usando esta maneira de declarar o nosso método, o nome Pizza nunca é diretamente referenciado e herança e método de substituição vai funcionar perfeitamente
```python
class Pizza(object):
    def __init__(self, radius, height):
        self.radius = radius
        self.height = height
 
    @staticmethod
    def compute_area(radius):
         return math.pi * (radius ** 2)
 
    @classmethod
    def compute_volume(cls, height, radius):
         return height * cls.compute_area(radius)
 
    def get_volume(self):
        return self.compute_volume(self.height, self.radius)
```

## Heranças Múltiplas ? Utilize mix-in

As melhores práticam ditam para evitar o uso de Herança Múltipla. Dependendo da rede de classes e de como foram criadas, podem ocorrer comportamentos sem controle do programador.

Porém, programadores Python podem utilizar o conceito de __Mix-in__, uma minúscula classe que define unicamente um conjunto de métodos adicionais que uma classe pode oferecer. Essa classes são simples e nem construtor deve ser chamado.

A inspecção dinãmica do Python permite escrever funcionalidade genérica uma unica vez em um mix-in, que pode ser aplicada em muitas outras classes.

Por exemplo, podemos converter um objeto em um JSON e um JSON em objeto para enviar/receber por requisições WEB. Podemos criar um mixin com essa funcionalidade e extender nossas classes.

```python
class JsonMixin(object):
     
    @classmethod
    def from_json(cls, data):
        kwargs = json.loads(data)
        return cls(**kwargs)
        
    def to_json(self):
        return json.dumps(self.to_dict())
        
...

class Pedido(JsonMixin): ...
```

Os mixin-ns pode ser combinados, tornando simples criar hierarquias de classes utilitárias, por exemplo, podemos criar Mix-in para controlar data de cadastro e outro para log de alterações de registros das classes ou slug, ou qualquer outra funcionalidade.

```python
class Pedido(JsonMixin, CreateMixin, LogMixin): ...
```

__Lembre-se:__ Evite herânças multiplas, empregue os mixins que fazem a mesma funcionalidade, porem como plugins que fornecem novos comportamentos personalizáveis. Combine dois ou mais mix-ins para criar funcionalidades complexas a partir de comportamentos simples.
