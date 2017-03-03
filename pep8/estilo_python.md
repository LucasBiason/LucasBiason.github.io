# Saiba sobre PEP8 e Codificação

## Siga o guia da PEP8

É muito importante seguir o estilo da PEP8, com ele seu código será mais legivel e facil de dar manutenção. Isso tambem evita possíveis erros de sintaxe ou de lógica.

**1. EditorConfig:** uso de um editor de texto com EditorConfig suporte para
evitar o recuo e as questões de espaço em branco.

**2. Indentação:** usar 4 espaço por nível de indentação´, é o mais recomendado. **Nunca misture espaços com tabulação.**

**3. Comprimento máximo de linhas:** limitar todas as linhas a um máximo de  80
caractere. Isso torna o código mais legivel e facil de entender, linhas gigantes significam que existem muitas funções sendo executadas na mesma linha (e que podem ser quebradas em várias, deixando simples de entender) ou identificadores com nome grandes (tente reduzir essas nomes)

**4. Linhas em branco:**
- Separe funções e definições de classe com duas linhas em branco.
- Métodos dentro de uma classe devem ser separados com uma única linha em branco.
- Linhas extras podem ser usadas (esporadicamente) para separar grupos de funções relacionadas

**5. Import:**
- Imports devem sempre ser feitos em linhas separadas.
- Imports devem ser sempre colocados no topo do arquivo, logo depois de quaisquercomentários ou docstrings, e antes de constantes ou globais (Salvo necessidade extrema de colocar em funções, para evitar loop de importação, nesse caso, coloque sempre no inicio na função, após o Docstring). Eles devem ser agrupados seguindo a ordem:

        1º módulos da biblioteca padrão
        2º módulos grandes relacionados entre si (por exemplo, todos os módulos de
            e-mail usados pela aplicação).
        3º módulos específicos da aplicação
        OBS: colocar uma linha em branco separando cada grupo de módulos

- Dica:
```python
        # Não use:
        import sys, os

        # E sim:
        import sys
        import os
```
**6. Espaços em expressões e instruções:**
- Não deve ter espaços antes e após parêntese, colchete ou chave.
- Não deve ter espaços antes de uma vírgula, ponto-e-vírgula ou dois-pontos.
- Não deve ter espaços antes do parêntese que abre a lista de argumentos de uma função.
- Não deve ter espaços antes da chave que abre um índice.
- Não deve ter espaços a mais do que um espaço em volta de algum operador, para alinhar os operandos.
```python
        # Não use:
        spam( ham[ 1 ], { eggs: 2 } )
        if x == 4 : print x , y ; x , y = y , x
        spam (1)
        dict ['key'] = list [index]
        x             = 1
        y             = 2
        long_variable = 3

        # E sim:
        spam(ham[1], {eggs: 2})
        if x == 4: 
            print x, y; 
            x, y = y, x
        spam(1)
        dict['key'] = list[index]
        x = 1
        y = 2
        long_variable = 3
```
- Sempre circunde os seguintes operadores binários com um único espaço de cada lado: =, ==, <, >, !=, <>, <=, >=, in, not in, is, and, or, not.
- Por convenção usaremos espaços entre operadores aritméticos, como em:
```python
        submitted = submitted + 1
        c = (a+b) * (a-b)
```
- Não use espaços ao redor do sinal de igual (=) quando usado para indicar um valor padrão de um argumento. Faça assim, por exemplo:
```python
        def complex(real, imag=0.0):
            return magic(r=real, i=imag)
```
- Múltiplos comandos na mesma linha: Nunca!

**7. Comentários:**
- Sempre tenha como prioridade manter os comentários atualizados com as mudanças no código.
- Comentários devem sempre ser frases completas e sua primeira letra deve ser maiúscula.
- Comentários em bloco devem ser indentados no mesmo nível do código a que se referem.
- Cada linha deve começar com # e um espaço (a menos que o texto dentro do comentário seja indentado)
- Parágrafos dentro de um bloco devem ser separados por uma linha contendo uma única tralha #.
- O bloco inteiro deve ser separado por uma linha em branco no topo e embaixo.
- Comentários na mesma linha devem ser usados esporadicamente. Devem ser separados do comando por pelo menos dois espaços. Como outros comentários, devem começar com uma tralha e um espaço.
- Escrever seus comentários em inglês.

**8. Docstrings:**
- A PEP 257 descreve as convenções usadas para docstrings.
- Escreva docstrings para todo módulo, função, classe e método público.
- Uma linha são para casos muito evidentes Eles realmente deve caber em uma linha. Não há nenhuma linha em branco antes ou após a docstring quando utiliza somente uma linha. Por exemplo:
```python
        def kos_root():
            """Return the pathname of the KOS root directory."""
            global _kos_root
            if _kos_root:
                return _kos_root
            ...
```
- O docstring é uma frase que termina em um período. Prescreve a função ou o efeito do método como um comando ( "Faça isso", "Return isso"), e não como uma descrição; por exemplo, não escreva "Retorna o nome do caminho ...".
- Docstrings multi-linhas consistem de uma linha de resumo como uma Docstring uma linha, seguido por uma linha em branco, seguida por uma descrição mais elaborada. A linha de resumo deve estar na mesma linha que as aspas de abertura.
```python
        def complex(real=0.0, imag=0.0):
            """Form a complex number.

            Keyword arguments:
            real -- the real part (default 0.0)
            imag -- the imaginary part (default 0.0)
            """
            if imag == 0.0 and real == 0.0:
                return complex_zero
            ...
```
**6. Nomes e Identificadores:**
- Nomes a evitar Nunca use os caracteres 'l' (L minúsculo), 'O' (o maiúsculo) ou 'I' (i maiúsculo) sozinhos como nomes de variáveis. Em algumas fontes, esses caracteres são indistinguíveis dos números um e zero. Quando tentado a usar somente 'l', use 'L'.
- Nomes de Módulos: Módulos devem usar o padrão de PalavrasComeçandoPorMaiúscula.
- Nomes de Classes: Classes devem usar o padrão de PalavrasComeçandoPorMaiúscula.
- Nomes de Exceptions: Se um módulo define uma única exception usada para todos os tipos de erro, ela é geralmente chamada "Error".
- Nomes de Funções: Funções globais, exportadas por um módulo usar o padrão PalavrasComeçandoPorMaiúscula.
- Variáveis Globais: devem ser usadas somente dentro do módulo. As convenções são as mesmas para funções. Módulos que são projetados para ser usados com 'from M import *' devem ter suas globais com um underscore como prefixo, para evitar que sejam exportadas.

## Recomendações ao Programar:
- Comparações com singletons, como None, False e True devem sempre ser feitas com "is" ou "is not".
- Cuidado para não escrever "if x" quando o que você deseja é "if x is not None", como ao testar se uma variável ou argumento que tem um valor padrão de None, teve outro valor atribuído.
- Classes são sempre preferidas à strings, como em exceptions. Módulos ou pacotes devem definir sua própria classe-exception base, que deve ser uma subclasse da classe Exception. Sempre inclua uma docstring. Exemplo:
```python
        class MessageError(Exception):
            """Base class for errors in the email package."""
```
- Evite fatiar strings quando verificando prefixos ou sufixos. Use os métodos startswith() e endswith(), que são mais eficientes e menos sujeitos a erro. Por exemplo:
```python
        # Não use:
        if foo[:3] == 'bar':

        # E sim:
        if foo.startswith('bar'):
```
- Não compare valores booleanos com True e False usando == (o tipo Bool é novo em Python 2.3)
```python
        # Não use:
        if greeting == True:

        # E sim:
        if greeting:
```
- Quando estiver verificando se o objeto é uma string, lembre-se que ele também pode ser uma string unicode! Em Python 2.3, str e unicode têm uma classe base em comum, basestring, então você pode simplesmente escrever:
```python
        if isinstance(obj, basestring):
```
## Saiba as diferenças entre bytes, str, unicode

**No Python 3:** instâncias de **bytes** contêm valores primérios de 8 bits. Instâncias de **str** conêm caracteres Unicode.

**No Python 2:** instâncias de **str** contêm valores primérios de 8 bits. Instâncias de **unicode** conêm caracteres Unicode.

É importante observar que as instâncias de **str** em Python 3 e unicode em Python 2 não têm uma codificação binária associada. 
Para converter caracteres Unicode para dados binários, é preciso usar o método **encode**. 
Para converter dados binários para caracteres Unicode, é preciso usar o método **decode**

**Dica:** o núcleo do seu programa deve usar apenas caracteres Unicode (**str** no Python3, **unicode** no Python 2) e não devem fazer nenhum julgamento quanto a condificação deles

**Dica:** em Python 3, as operaçoes envolvendo arquivos sempre esperam codificação UTF-8, no 2, o default é sempre binário.
No Python2, o seguinte código funciona, porém, causa erro no Python3:
```python
with open('/tmp/arquivo.txt','w') as f:
```
A causa é o encoding adicionado no Python 3 para o **open**. Para que funcione:
```python
with open('/tmp/arquivo.txt','wb') as f:
```



###### Links úteis:

:clipboard: [Unicode em Python 2](https://docs.python.org/2/howto/unicode.html)

:clipboard: [Unicode em Python 3](https://docs.python.org/3/howto/unicode.html)

:clipboard: [Python UnicodeEncodeError: 'ascii' codec can't encode character](http://www.saltycrane.com/blog/2008/11/python-unicodeencodeerror-ascii-codec-cant-encode-character/)

============================
:arrow_left: [Voltar](https://github.com/LucasBiason/PadroesPython/blob/master/python_eficaz/boas_praticas.md)
