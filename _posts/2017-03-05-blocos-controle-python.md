---
layout: post
title: "Python Eficaz #001 - Dicas para Blocos de Controle"
description: "Algumas dicas para iniciantes de como usar Blocos de Controle em python."
tags: [python, exception, if/else]
modified: 2017-03-05
comments: true
image:
  feature: capas/python-eficaz.gif
---

**Dica:** Evite usar blocos else depois de laços for e White.
Else é chamado imediatamente após a execução do laço " faça isso se o bloco anterior não falhou ". Isso pode causa uma má interpretação do código. Fora que inserir um comando break em um laço faz com que o bloco seja ignorado.

Os blocos else são úteis em laços que procuram por alguma informação. **Pois será executado senão encontrar break.**

**Dica:** Todo ‘elif‘ tem um irmão ‘else‘.
Sempre que você precisar usar uma construção if/elif coloque uma cláusula ‘else‘. Pode ser usado para gerar um “valor default” ou gerar uma exceção. Trabalhando todas as possibilidades nos ‘if/elif‘ e alertando a situação inválida.
```python
def processo(comando):
   if comando == "x":
      ....
   elif comando == "y":
      ....
   elif comando == "z":
      ...
   else:
      raise Exception(u"Comando %s inválido." % (comando,))
```

**Dica:** Aprendendo a usar try/except/else/finally

* Use try/finally quando quiser que as exceções sejam propagadas para os níveis superiores, mas também quiser rodar um código de limpeza quando a exceção ocorrer (pode existir ou não exceções a serem tratadas). Exemplo: um uso comum é para assegurar fechamento de manipuladores de arquivo.
```python 
handle = open("meu_arquivo.txt")
try: 
    data = handle.read()
finally:
    handle.close()
```

* Use try/except/else para deixar claro quais exceções serão tratadas pelo seu código e quais serão propagadas para os níveis superiores. Quando o bloco try não gera uma exceção, o bloco else será executado. O bloco else ajuda a minimizar a quantidade de código no bloco try e melhora a legibilidade.
```python
def get_key(data, key):
    try:
        valor = json.loads(data) # pode gerar ValueError
    except ValueError as e:
        raise KeyError from e
    else:
        return valor[key] # pode gerar KeyError
```

* Use try/except/else/finally (tente/exceto se/senão/finalmente) quando quiser fazer tudo isso em uma unica estrutura composta.

* O else ajuda a minimizar a quantidade de código nos blocos try e visualmente separar o resultado verdadeiro dos erros gerados. Pode ser usado para executar ações adicionais depois que um bloco try com resultado positivo, mas antes da limpeza comum a todos os resultados feita pelo bloco finally.

**Dica:** __Deixem as exceções fluirem.__

A menos que você saiba exatamente o que você deve fazer quando uma exceção aparece deixe-a “subir”. Pode ser que “lá em cima” alguém saiba cuidar dela adequadamente.

Quando não fazemos isso estamos ocultando informação importante para os usuários do nosso código (sejam eles usuários, outros desenvolvedores ou nós mesmos), pois a excessão seria tratada de forma correta, e quem saberia tratar, nunca terá o que fazer. No lugar disso:
```python
def f():
   try:
      return conecta()
   except ExcecaoQueDeveriaSerErro:
      return None
```
Implemente:
```python
def f():
   return conecta()
```

**Importante:** Nunca...
```python
 try:
     .....
 except:
    pass
```
Isso oculta possíveis bugs, pois faz o código executar e passa e simplesmente ignorar quando da erro, sem dar um alerta ou tentar corrigir o problema. Seria como se vc não acertasse no ponto da massa de um bolo e mesmo assim seguisse adiante para cortar e rechear.. la na frente... você terá uma meleca.. não um bolo.
