# Quando Utilizar Classes  

Aqui veremos alguns tópicos sobre quando utilizar classes

## Registros Complexos com Dicionários x Classes

Ao mantermos dados, podemos fazê-lo de duas formas:

i. Dicionário:
```python
   vagas = []
   
   vagas.append({
       'Description' : "Auxiliar de Limpeza",
       'qtde' : 5
   }
    vagas.append({
       'Description' : "Assistênte Administrativo",
       'qtde' : 1
   }
   ...
```

Dicionário são tão fáceis de usar que existe o risco de estendê-los demais e escrever um código de dificil manutenção e frágil.
Imagina que iriamos precisar colocar uma Empresa nesse dicionário, que também  um dicionário usando código, e precisariamos de um endereço que esta vinculado a empresa.

```python
   empresas = [{
       'name' : "Juca LTDA",
       'id' : 1,
       'logradouro': 'Rua 123'
   },{
       'name' : "Eletrônicos Maria LTDA",
       'id' : 2,
       'logradouro': 'Rua UIJ'
   },
   ]
   
   vagas = [{
       'Description' : "Auxiliar de Limpeza",
       'qtde' : 5,
       'empresa' : 2
   },{
       'Description' : "Assistênte Administrativo",
       'qtde' : 1,
       'empresa' : 1
   }
   ]
   
   // precisariamos fazer:
   empresa_vaga = vagas[0][empresa]
   for empresa in empresas:
      if empresa_vaga = empresa["id']:
          endereco = empresa["logradouro']
   ...

```

Agora imagina que precisaríamos vincular mais informações e realizar pesquisas com esses dados.  Por Exemplo:

```python
   empresas = [{
       'name' : "Juca LTDA",
       'id' : 1,
       'logradouro': {
          'rua' :...,
          'numero': ...,
          'cidade':...,
          'estado:'...
       }
   },{
       'name' : "Eletrônicos Maria LTDA",
       'id' : 2,
       'logradouro':  {
          'rua' :...,
          'numero': ...,
          'cidade':...,
          'estado:'...
       }
   },
   ]
   
   
   // precisariamos fazer:
   empresa_vaga = vagas[0][empresa]
   for empresa in empresas:
      if empresa_vaga = empresa["id']:
          endereco = empresa["logradouro'][rua]
             
```

Quando a complexidade do uso de dicionários começa a ficar grande, é melhor utilizarmos classes.

ii. Classe:
```python
   class Logradouro(objects):
       def __init__(self, rua=None, numero=None, cidade=None, estado=None):
           self._rua = rua
           self._numero = numero
           self._cidade = cidade
           self._estado = estado
        
        @property
        def rua(self):
            return rua
            
   class Empresa(objects):
       def __init__(self, name=None, id=None, logradouro=None):
           self._name = name
           self._id = id
           self._logradouro = logradouro
                       
        @property
        def rua(self):
            return self._logradouro.rua
           
   class Vaga(object):
       def __init__(self, description=None, qtde=None, empresa=None):
           self._description = description
           self._qtde = qtde
           self._empresa = empresa
       
        @property
        def rua(self):
            return self._empresa.rua
            
            
   // precisariamos fazer:
   rua_x = Logradouro(rua='Rua 123')
   
   empresa = Empresa(
       name="Juca LTDA",
       id=1,
       logradouro=rua_x
   )
   
   vaga = Vaga(
       description = "Auxiliar de Limpeza",
       qtde = 5,
       empresa = empresa
   )
   
   endereco = vaga.rua
   
```

Podemos estender a complexidade a vontade, e criar mecanismos (property, methods, staticmethods,...) de acesso e pesquisa dos dados.

O exemplo dados foi relativamente simples, mas existem estruturas bem mais complexas já criadas em dicionários e que são caóticas para dar manutenção ou estender. Nesses casos, refatore seu código para uma hierarquia de classes auxiliares 

Evite criar dicionários com valores que sejam outros dicionários internos e que podem começar a ficar mais complexos.

## Funções x Classes

Em vez de definir e instanciar classes, as funções são normalmente tudo o que precisamos para interfaces simples entre componentes do Python. No lugar de criar uma estrutura de classes e métodos, uma fução simples pode ser mais facil (menos código e rápido para fazer) e rápido (melhor desempenho do que ficar criando instaâncias de objetos da classe).

Referências a funções e métodos em Python são de primeira classe, o que significa poderem ser usadas em expressões como qualquer outro tipo de dado.


============================
:arrow_left: [Voltar](https://github.com/LucasBiason/PadroesPython/blob/master/python_eficaz/boas_praticas.md)
