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


* Serializador -> Serve para fazer a conversão de dados complexos em um datatype python.
Utilizar o ModelSerializer 

* Criar um arquivo dentro da pasta escola chamado serializers.py 
    No arquivo codar :   from rest_framework import serializers
from escola.models import Estudante, Curso

class EstudanteSerializer(serializers.ModelSerializer):
    class meta:
        model = Estudante
        fields = ['id','nome','email','cpf','data_nascimento','celular']


class CursoSerializer(serializers.ModelSerializer):
    class meta:
        model = Curso
        fields = '__all__'


No terminal importar

    from escola.models import Estudante
    from escola.serializers import EstudanteSerializer
    querySet =  Estudante.objects.all()
    querySet
    serializador = EstudanteSerializer(querySet, many = True)


* Construir view set

Agrupado de views que contém operações CRUD. Ler, editar, criar

No arquivo views.py inserir : 

Importar as models Estudante e Curso -> from escola.models import Estudante, Curso
Importar cada serializer -> from escola.serializers import EstudanteSerializer, CursoSerializer
from rest_frameworks import viewsets


Criar a classe EstudanteViewSet(viewsets.ModelViewSet):
    queryset = Estudante.objects.all()
    serializer_class = EstudanteSerializer


    CursoViewSet(viewsets.ModelViewSet):
    queryset = Curso.objects.all()
    serializer_class = CursoSerializer


* Routers - > Criar um objeto da classe Router que armazenará todos os objetos da rota

*   Construir a model Matricula. 

class Matricula(models.Model):

    PERIODO = (
        ('M', 'Matutino'),
        ('V', 'Vespertino'),
        ('N', 'Noturno')
    )

    estudante = models.ForeignKey(Estudante, on_delete = models.CASCADE)
    curso = models.ForeignKey(Curso, on_delete = models.CASCADE)
    peridodo =  models.CharField(max_length = 1, choices = PERIODO , blank = False, null = False, default = 'B')

    Depois rodar o comando python manage.py make migrations depois python manage.py migrate


* Agora que a model de matricula está feita, criar a serializer dela.
