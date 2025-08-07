# Crud-de-estudiante-simple
Proyecto: CRUD estudiante.. Este sistema utiliza Django, HTML, CSS. A quién va dirigido: Estudiantes de preparatoria Nivel: principiante.
Aquí tienes el **procedimiento completo y detallado** para desarrollar el **proyecto CRUD Estudiante** utilizando **Django, HTML y CSS**, dirigido a estudiantes de preparatoria (nivel principiante). Todo está organizado paso a paso para que puedas seguirlo sin problemas.

---

### 📁 Estructura Final del Proyecto
```
crud_estudiante/
├── Backend_cbtis/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
├── app_estudiante/
│   ├── __init__.py
│   ├── models.py
│   ├── views.py
│   ├── admin.py
│   ├── urls.py
│   └── templates/
│       ├── listar_estudiantes.html
│       ├── agregar_estudiante.html
│       ├── editar_estudiante.html
│       └── borrar_estudiante.html
├── manage.py
└── venv/  (entorno virtual)
```

---

## ✅ PASO 1: Crear y Activar el Entorno Virtual

```bash
# Crear entorno virtual
python -m venv venv

# Activar entorno virtual en Windows
venv\Scripts\activate

# Activar entorno virtual en macOS/Linux
source venv/bin/activate
```

> ✅ Verifica que aparezca `(venv)` al inicio de tu terminal.

---

## ✅ PASO 2: Seleccionar el Intérprete de Python

En tu editor (por ejemplo, **VS Code**):

1. Abre el proyecto en VS Code.
2. Presiona `Ctrl + Shift + P` → Escribe "Python: Select Interpreter".
3. Elige el que diga algo como:  
   `./venv/Scripts/python.exe` (Windows) o `./venv/bin/python` (macOS/Linux).

---

## ✅ PASO 3: Instalar Django

```bash
pip install django
```

> ✅ Puedes verificar con: `python -m django --version`

---

## ✅ PASO 4: Crear el Proyecto Django "Backend_cbtis"

```bash
django-admin startproject Backend_cbtis .
```

> ⚠️ Asegúrate de estar en la carpeta `crud_estudiante` cuando ejecutes este comando (el punto final `.` es importante).

---

## ✅ PASO 5: Crear la Aplicación "app_estudiante"

```bash
python manage.py startapp app_estudiante
```

---

## ✅ PASO 6: Configurar `settings.py` (Backend_cbtis/settings.py)

Abre: `Backend_cbtis/settings.py`

### 1. Agregar `'app_estudiante'` a `INSTALLED_APPS`:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app_estudiante',  # ← Agrega esta línea
]
```

### 2. Configurar templates (opcional, ya que Django lo busca automáticamente en apps)

> No necesitas hacer cambios adicionales si usas la carpeta `templates` dentro de la app.

---

## ✅ PASO 7: Configurar URLs del Proyecto (Backend_cbtis/urls.py)

Reemplaza el contenido de `Backend_cbtis/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('app_estudiante.urls')),  # Incluye las URLs de la app
]
```

---

## ✅ PASO 8: Crear `urls.py` en la App `app_estudiante`

Crea el archivo: `app_estudiante/urls.py`

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='inicio'),
    path('<int:id>', views.ver_estudiante, name='ver_estudiante'),
    path('agregar/', views.agregar_estudiante, name='agregar_estudiante'),
    path('editar/<int:id>/', views.editar_estudiante, name='editar_estudiante'),
    path('borrar/<int:id>/', views.borrar_estudiante, name='borrar_estudiante'),
]
```

---

## ✅ PASO 9: Definir el Modelo (app_estudiante/models.py)

Ya lo tienes, pero aquí está por si necesitas copiarlo:

```python
from django.db import models

class Estudiante(models.Model):
    nombre = models.CharField(max_length=50)
    telefono = models.CharField(max_length=10)

    def __str__(self):
        return f'Estudiante: {self.nombre} {self.telefono}'
```

---

## ✅ PASO 10: Hacer Migraciones

```bash
# Crear migración (crea el archivo SQL basado en models.py)
python manage.py makemigrations

# Aplicar migración (crea la tabla en la base de datos)
python manage.py migrate
```

---

## ✅ PASO 11: Registrar el Modelo en Admin (app_estudiante/admin.py)

Abre `app_estudiante/admin.py`:

```python
from django.contrib import admin
from .models import Estudiante

admin.site.register(Estudiante)
```

---

## ✅ PASO 12: Crear Superusuario

```bash
python manage.py createsuperuser
```

> Sigue las instrucciones: ingresa nombre de usuario, correo (opcional) y contraseña.

---

## ✅ PASO 13: Crear la Carpeta `templates` y los Archivos HTML

Dentro de `app_estudiante`, crea:

```
app_estudiante/
└── templates/
    ├── listar_estudiantes.html
    ├── agregar_estudiante.html
    ├── editar_estudiante.html
    └── borrar_estudiante.html
```

---

### 📄 1. `listar_estudiantes.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Lista Alumnos</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
        th, td { border: 1px solid #ccc; padding: 8px; text-align: left; }
        th { background-color: #f2f2f2; }
        .btn { margin: 5px; padding: 5px 10px; text-decoration: none; color: white; }
        .editar { background-color: #007BFF; }
        .borrar { background-color: #DC3545; }
        .agregar { background-color: #28A745; display: inline-block; margin: 10px 0; padding: 10px 15px; color: white; text-decoration: none; }
    </style>
</head>
<body>
    <h1>Listado de alumnos</h1>
    <table>
        <thead>
            <tr>
                <th>Nombre</th>
                <th>Teléfono</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            {% for estudiante in estudiantes %}
            <tr>
                <td>{{ estudiante.nombre }}</td>
                <td>{{ estudiante.telefono }}</td>
                <td>
                    <a href="/editar/{{ estudiante.id }}/" class="btn editar">Editar</a>
                    <a href="/borrar/{{ estudiante.id }}/" class="btn borrar">Borrar</a>
                </td>
            </tr>
            {% empty %}
            <tr>
                <td colspan="3">No hay estudiantes registrados.</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
    <a href="/agregar/" class="btn agregar">Agregar Estudiante</a>
</body>
</html>
```

---

### 📄 2. `agregar_estudiante.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Agregar Estudiante</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        form { max-width: 400px; }
        label { display: block; margin: 10px 0 5px; }
        input { width: 100%; padding: 8px; margin-bottom: 10px; }
        button { background-color: #007BFF; color: white; padding: 10px 15px; border: none; cursor: pointer; }
        button:hover { background-color: #0056b3; }
    </style>
</head>
<body>
    <h1>Agregar Estudiante</h1>
    <form method="post">
        {% csrf_token %}
        <label>Nombre:</label>
        <input type="text" name="nombre" required>

        <label>Teléfono:</label>
        <input type="text" name="telefono" maxlength="10" required>

        <button type="submit">Guardar</button>
    </form>
    <a href="/">Volver al listado</a>
</body>
</html>
```

---

### 📄 3. `editar_estudiante.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Editar Estudiante</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        form { max-width: 400px; }
        label { display: block; margin: 10px 0 5px; }
        input { width: 100%; padding: 8px; margin-bottom: 10px; }
        button { background-color: #007BFF; color: white; padding: 10px 15px; border: none; cursor: pointer; }
        button:hover { background-color: #0056b3; }
    </style>
</head>
<body>
    <h1>Editar Estudiante</h1>
    <form method="post">
        {% csrf_token %}
        <label>Nombre:</label>
        <input type="text" name="nombre" value="{{ estudiante.nombre }}" required>

        <label>Teléfono:</label>
        <input type="text" name="telefono" value="{{ estudiante.telefono }}" maxlength="10" required>

        <button type="submit">Actualizar</button>
    </form>
    <a href="/">Volver al listado</a>
</body>
</html>
```

---

### 📄 4. `borrar_estudiante.html`

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Confirmar Borrado</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        .btn { padding: 10px 15px; text-decoration: none; color: white; margin: 5px; }
        .confirmar { background-color: #DC3545; }
        .cancelar { background-color: #6c757d; }
    </style>
</head>
<body>
    <h1>¿Estás seguro de borrar a {{ estudiante.nombre }}?</h1>
    <p>Teléfono: {{ estudiante.telefono }}</p>
    <form method="post">
        {% csrf_token %}
        <button type="submit" class="btn confirmar">Sí, Borrar</button>
    </form>
    <a href="/" class="btn cancelar">Cancelar</a>
</body>
</html>
```

---

## ✅ PASO 14: Crear las Vistas (app_estudiante/views.py)

Reemplaza el contenido de `app_estudiante/views.py`:

```python
from django.shortcuts import render, redirect, get_object_or_404
from .models import Estudiante

# Listar estudiantes
def index(request):
    estudiantes = Estudiante.objects.all()
    return render(request, 'listar_estudiantes.html', {'estudiantes': estudiantes})

# Ver estudiante (opcional, puedes usarlo si quieres detalle)
def ver_estudiante(request, id):
    estudiante = get_object_or_404(Estudiante, id=id)
    return render(request, 'ver_estudiante.html', {'estudiante': estudiante})

# Agregar estudiante
def agregar_estudiante(request):
    if request.method == 'POST':
        nombre = request.POST['nombre']
        telefono = request.POST['telefono']
        Estudiante.objects.create(nombre=nombre, telefono=telefono)
        return redirect('inicio')
    return render(request, 'agregar_estudiante.html')

# Editar estudiante
def editar_estudiante(request, id):
    estudiante = get_object_or_404(Estudiante, id=id)
    if request.method == 'POST':
        estudiante.nombre = request.POST['nombre']
        estudiante.telefono = request.POST['telefono']
        estudiante.save()
        return redirect('inicio')
    return render(request, 'editar_estudiante.html', {'estudiante': estudiante})

# Borrar estudiante
def borrar_estudiante(request, id):
    estudiante = get_object_or_404(Estudiante, id=id)
    if request.method == 'POST':
        estudiante.delete()
        return redirect('inicio')
    return render(request, 'borrar_estudiante.html', {'estudiante': estudiante})
```

> ⚠️ Nota: No usamos `forms.py`, tal como se indicó.

---

## ✅ PASO 15: Ejecutar el Servidor

```bash
python manage.py runserver
```

> Abre tu navegador y ve a: `http://127.0.0.1:8000`

---

## ✅ PASO 16: Acceder al Panel de Administración

1. Ejecuta el servidor si no está corriendo.
2. Ve a: `http://127.0.0.1:8000/admin`
3. Inicia sesión con el superusuario creado.

---

## ✅ PASO 17: Parar el Servidor

Presiona en la terminal:

```bash
Ctrl + C
```

> Para salir del servidor en ejecución.

---

## ✅ Resumen de Comandos Frecuentes

| Acción | Comando |
|-------|--------|
| Activar entorno | `venv\Scripts\activate` (Win) / `source venv/bin/activate` (macOS/Linux) |
| Instalar Django | `pip install django` |
| Crear proyecto | `django-admin startproject Backend_cbtis .` |
| Crear app | `python manage.py startapp app_estudiante` |
| Hacer migraciones | `python manage.py makemigrations` |
| Aplicar migraciones | `python manage.py migrate` |
| Crear superusuario | `python manage.py createsuperuser` |
| Ejecutar servidor | `python manage.py runserver` |
| Parar servidor | `Ctrl + C` |

---

✅ **¡Listo!** Has creado un sistema CRUD completo para estudiantes con Django, usando solo vistas basadas en funciones y HTML/CSS básico. Ideal para nivel principiante.

¿Quieres que te genere un archivo `.zip` estructurado o un script de instalación? Puedo ayudarte también.
