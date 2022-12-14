# Django
## Inicialización del proyecto 
Django es un framework para desarrollo web escrito en python, por lo que las instrucciones deben de ser ejecutadas en una terminal de python.
Para crear un nuevo proyecto se usa la instrucción: 

	django-admin startproject Name

En este caso se nombro Tutorial, lo que genera un carpeta con el nombre que se le asigno y las funciones para que inicie el servidor.

Para iniciar el servidor se usa la instrucción:

	manage.py runserver


>[!Note]
> En mi caso para correr este comando en terminal debo llamar a python desde la capeta raiz y despues usar el path para finalmente correr el programa:   
	& C:/Users/loboa/AppData/Local/Programs/Python/Python39/python.exe c:/Users/loboa/OneDrive/Documentos/SS/Tutorial/manage.py runserver

![image](https://user-images.githubusercontent.com/57508332/198139596-7dd3a842-79b4-47ce-a0b6-7f44672ba645.png)

## Iniciar app
Para generar el sitio web y sus demas funciones necesitamos crear una app, esto se realiza con el comando `manage.py startapp Name`, en este caso la app se llamó libreria.
Este comando genera una carpeta dentro de la carpeta principal del proyecto con el nombre que se le haya dado.
>[!Caution] Precaución 
>	Para poder hacer uso de la nueva app hay que declararla en el programa `settings.py` en la seccion Aplication definition que esta en la carpeta con el mismo nombre que el proyecto `\proyecto\proyecto\settings.py`
```Python
	# Application definition
	INSTALLED_APPS = [	
	    'django.contrib.admin',
	    'django.contrib.auth',
	    'django.contrib.contenttypes',
	    'django.contrib.sessions',
	    'django.contrib.messages',
	    'django.contrib.staticfiles',
	    'libreria',
	]
```

## Añadir vistas a la pagina web
El manejo de las vistas se hace mediante funciones que se declaran en `\proyecto\newapp\views.py` en donde se añaden los modulos necesarios como _HttpResponse_, estas vistas contienen las llamadas a los archivos `.html` que contienen las vistas de la pagina.

La gestion de las direcciones web se hace en `\proyecto\newapp\urls.py` donde se añaden las direcciones web y la vista asociada a esa direccion.
>[!Danger]
>Hay dos archivos `urls.py` uno ubicado en `\proyecto\proyecto\` y otro en `\proyecto\newapp\`, el que esta dentro de la app creada no existe por lo que hay que crearlo y añadir el siguiente codigo:
```Python
	from django.urls import path
	from . import views`
	urlpatterns = [
	path('', views.inicio, name = 'inicio'),
	]
```
> En el archivo `\proyecto\proyecto\urls.py` hay que añadir en urlpatterns el nuevo archivo creado en la app
```Python
path('', include('newapp.urls')),
```
> Ademas tambien hay que importar el comando `include`
```Python
from django.urls import include
```

### Carpeta templates
Las archivos `.html` se guardan en una carpeta llamada templates en la app creada, para el ejemplo se realizo un archivo plantilla (`base.html`) que sirve de base para las vistas asi como las vistas.
Se crearon las siguientes vistas:
- Inicio
- Nosotros
- Libros
	- Editar
	- Crear
 ![image](https://user-images.githubusercontent.com/57508332/198154648-8d9f2d6d-b80a-41e6-8ae9-d3e63f4db544.png)
 ![image](https://user-images.githubusercontent.com/57508332/198154666-c2932900-a9c5-4367-9f64-9e6bcf3d493a.png)
 ![image](https://user-images.githubusercontent.com/57508332/198155438-46834027-0572-4d1a-a628-7d03f7f9a211.png)

## Migración 

Para realizar la migración se requiere que la base de datos exista y, tener un modelo creado y guardado en `models.py`
La creación de la base de datos se hace en MySQL Workbench
```MySQL
create database name;
```

En python necesitamos que la libreria `pymysql` este instalada en caso que no lo este se usa
```Terminal
pip install pymysql
```

En el archivo `proyecto/settings.py` hay que modificar el apartado de databases
```Python
DATABASES = {
	'default': {
		'ENGINE': 'django.db.backends.mysql',
		'NAME': 'eeg_db',
		'USER': 'root',
		'PASSWORD': 'qwerty',
		'HOST': '127.0.0.1',
		'PORT': '3306',
	}
}
```

Los modelos (tablas) se declaran en el archivo `models.py` 
>[!Note] 
> Para este ejemplo se uso el siguiente modelo
``` Python
from django.db import models
# Create your models here.
 class Libro(models.Model):
     id = models.IntegerField(primary_key=True)
     titulo = models.CharField(max_length=100, verbose_name='Titulo')
     imagen = models.ImageField(upload_to='imagenes/', null=True, verbose_name='imagen')      descripcion = models.TextField(null=True, verbose_name='descripcion') 
```

En el archivo `__init__.py` hay que añadir
``` Python
import pymysql
pymysql.install_as_MySQLdb()
```

Para realizar las migraciones hay que introducir en una terminal de python 
```
manage.py makemigrations
```

```
manage.py migrate
```

Una vez se crean los modelos, estos se declaran en el programa `admin.py` de la app:
```Python
from django.contrib import admin
from .models import *
# Register your models here.
admin.site.register(Libro)
```

### Usuario y contraseña de la base de datos
Una vez se tiene la estructura se crea el usuario para la base de datos usando una terminal de python
``` Terminal
 & C:/Users/loboa/AppData/Local/Programs/Python/Python39/python.exe c:/Users/loboa/OneDrive/Documentos/SS/Tutorial/manage.py createsuperuser
```
Lo cual nos pedira un usuario, correo y contraseña.

>[!Warning]
>Aqui SQL puede der lata, si no esta bien configurado el puerto de la base de datos, si el servicio esta detenido en la maquina o si hay dos servicios entrando apor el mismo puerto.

Despues de creado el usuario, se puede acceder a la base de datos con la ruta `/admin`

![image](https://user-images.githubusercontent.com/57508332/199045344-c8ae5d34-eb40-4d77-839e-59425de9673a.png)
![image](https://user-images.githubusercontent.com/57508332/199045476-bac43550-b75b-47c4-89a6-6e1a29e16491.png)


