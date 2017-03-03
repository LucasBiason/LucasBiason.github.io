
## Formatação do Código:

**1. Template style:**
- No código do modelo Django, coloque um (e apenas um) espaço entre as chaves
e o conteúdo da tag.

        Não use:
        {{foo}}

        E sim:
        {{ foo }}

**2. View style:**
- O primeiro parâmetro em uma função de visualização deve ser chamado request.

        Não use:
        def my_view(req, foo):
            # ...

        E sim:
        def my_view(request, foo):
            # ...

**3. Model style:**
- Os nomes de campo devem estar em minúsculas, usando sublinhados em vez de
camelCase.
- A class Meta deve aparecer após os campos são definidos, com uma única linha

        Não use:
        class Person(models.Model):
            FirstName = models.CharField(max_length=20)
            Last_Name = models.CharField(max_length=40)
            class Meta:
                verbose_name_plural = 'people'

        E sim:
        class Person(models.Model):
            first_name = models.CharField(
                max_length=20
            )
            last_name = models.CharField(
                max_length=40
            )

            class Meta:
                verbose_name_plural = 'people'

-  Classe meta:
        
        * db_table: Isso facilita a localização da Model que utiliza uma determinada tabela no banco.
        * verbose_name e verbose_name_plural: facilita log e alertas para emitir feedbacks para os usuários.
        
- Exemplo:
        
        class Meta:
            db_table = 'pessoa_fisica'
            verbose_name = _(u'Pessoa Física')
            verbose_name_plural = _(u'Pessoas Físicas')
            
-  Se você definir um __str__ método (anteriormente __unicode__ antes Python 3
foi suportado), decorar a classe modelo com python_2_unicode_compatible() .
- A ordem das classes internas do modelo e métodos padrão deve ser a seguinte
(observando que estes não são todos necessários):

        * Todos os campos de banco de dados
        * atributos gerente personalizado
        * class Meta
        * def __str__()
        * def save()
        * def get_absolute_url()
        * Quaisquer métodos personalizados

- Se choices é definida para um determinado campo de modelo, definir cada
opção como uma tupla de tuplas, com um nome all-maiúsculas como um atributo de
classe do modelo. Exemplo:

        class MyModel(models.Model):
            DIRECTION_UP = 'U'
            DIRECTION_DOWN = 'D'
            DIRECTION_CHOICES = (
                (DIRECTION_UP, 'Up'),
                (DIRECTION_DOWN, 'Down'),
            )


**4. APP style:**
- O conteúdo das apps deve ter um modelo parecido com:

        ----models (pasta)
        __init__.py (configurado para importar todas as models dos arquivos desse modulo, dessa forma podemos importar como : from nome_app.models import Model1, Model2, ... )
        model1.py
        model2.py
        ...
        ----forms (pasta opcional)
        __init__.py (configurado para importar todos os forms dos arquivos desse modulo, dessa forma podemos importar como : from nome_app.forms import Form1, Form2, ... )
        form1.py
        form2.py
        ...
        ----views (pasta)
        __init__.py (configurado para importar todas as views dos arquivos desse modulo, dessa forma podemos importar como : from nome_app.views import Views1, Views2, ... )
        view1.py
        view2.py
        ...
        ----methods (pasta opcional)
        __init__.py (configurado para importar todos os methods dos arquivos desse modulo, dessa forma podemos importar como : from nome_app.methods import Method1, Method2, ... )
        method1.py
        method2.py
        ...
        ----urls.py
        ----....outros arquivos gerais
