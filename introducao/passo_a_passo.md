* Entrar na pasta do projeto -> cd escola
* Digite o comando : python -m venv venv
* Adicione o caminho : venv\Scripts\activate.bat
* Instalar  a última versão do Django : pip install Django==5.1.2
* Construir o projeto : django-admin startproject setup .
* Criar o aplicativo : python manage.py startapp escola

* No código do arquivo **view.py**  

    1.  Importar from django.http import JsonResponse
    2.  Função para criar request.
        def estudante(request):
            if request.method == 'GET':
                estudante = {
                    'id'='1',
                    'nome'='felipe'
                }
                return JsonResponde(estudante) - > JsonResponse tranforma o tipo  que está em dicionário para json para colocar na API.

* Instalar o djangorestframework -> pip install djangorestframework
* instalar markdown -> pip install markdown
* pip freeze > requirements.txt  -> Instala todas as dependências
* Na pasta setup , no arquivo settings.py, adicionar 'escola' e 'rest_framework'

# Criando as models

1.  Estabelecer a classe -> class Estudante(models.Model):
                                nome = models.CharField(max_length = 100)
                                email = models.EmailField(blank = False, max_length = 30)
                                cpf = models.CharField(max_length = 11)
                                data_nascimento = models.DateField()
                                celular = models.CharField(max_length = 14)

                            def __str__(self):
                                return self.nome

                            class Curso(models.Models):
                                NIVEL = (
                                    ('B','Básico')
                                    ('I','Intermediário')
                                    ('A','Avançado')
                                )
                                codigo = models.CharField(max_length = 10)
                                descricao = models.CharField(blank = False, max_length = 100)
                                nivel = models.CharField(max_length = 1, choices = NIVEL , blank = False, null = False, default = 'B')
                                

                                def __str__(self):
                                    return self.codigo
                                
* Logo após terminal o modelo, é preciso fazer as migrations.
Para isso utiliza-se o comando -> python manage.py makemigrations.
Para então fazer a migração, utiliazr o comando python manage.py migrate

* Depois ir no arquivo admin e adicionar a classe Estudantes e Cursos

from django.contrib import admin

from escola.models import Estudante,Curso

class Estudantes(admin.ModelAdmin):
    list_display = ('id','nome','email','cpf','data_nascimento','celular')
    list_display_links = ('id','nome',)
    list_per_page = 20
    search_fields = ('nome',)

admin.site.register(Estudante,Estudantes)

class Cursos(admin.ModelAdmin):
    list_display = ('id','codigo','descricao')
    list_display_links = ('id','codigo',)
    search_fields = ('codigo',) # Pesquisar

* Logo após criar as classes no admin , criar um superusuário
Para fazer isso , digitar o comando : python manage.py createsuperuser

* Depois rodar o comando python manage.py runserver


