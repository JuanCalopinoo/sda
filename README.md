# Sistema de Gestión de Pedidos de Restaurante

Este proyecto tiene como objetivo implementar el módulo de Pedidos dentro de un sistema de gestión para un restaurante. El sistema está basado en un diagrama UML diseñado en la Unidad 1, que incluye los conceptos fundamentales de la programación orientada a objetos (POO), tales como la abstracción, encapsulación, herencia y polimorfismo.

## Estructura del Proyecto

El sistema está compuesto por 6 módulos principales, cada uno enfocado en diferentes aspectos del restaurante. Estos módulos son:

1. **Gestor de Clientes**: Maneja la información y las operaciones relacionadas con los clientes.
2. **Gestor de Mesas**: Administra la disponibilidad y asignación de mesas.
3. **Menú y Platos**: Permite la gestión de los platos ofrecidos por el restaurante.
4. **Personal del Restaurante**: Incluye la gestión del personal de cocina y meseros.
5. **Historial de Operaciones**: Almacena un registro de las operaciones realizadas.
6. **Módulo de Pedidos**: Responsable de gestionar los pedidos realizados por los clientes.

### Módulo de Pedidos

El módulo de Pedidos tiene las siguientes responsabilidades principales:

- **Gestión de Pedidos**: Registrar los pedidos realizados por los clientes, especificando los elementos solicitados, cantidades y observaciones.
- **Cálculo de Factura**: Calcular el total de los pedidos realizados.
- **Seguimiento de Estado**: Actualizar y visualizar el estado de los pedidos (e.g., EN_PREPARACION, SERVIDO, PAGADO).

### Implementación de POO

Los conceptos de abstracción y encapsulación se implementan a través de las clases principales que representan las entidades del sistema, tales como:

- **Cliente**: Representa a los clientes del restaurante.
- **Pedido**: Contiene información sobre los pedidos realizados.
- **ItemPedido**: Representa cada elemento del pedido.
- **PersonalCocina** y **Mesero**: Modelan al personal del restaurante.

El polimorfismo y la herencia también se emplean para garantizar que las interacciones entre clases sean flexibles y escalables.

## Diagrama UML

El siguiente diagrama UML describe la estructura del sistema. Este fue actualizado durante el desarrollo para reflejar los cambios realizados:

![Diagrama UML](https://github.com/user-attachments/assets/ae33e66d-7f59-4cef-b792-bfeb138b42ed)

El diagrama muestra las clases principales, sus atributos y métodos, así como las relaciones entre ellas, como agregaciones, composiciones y asociaciones.

## Uso del Sistema

1. **Registro de Clientes**: Los clientes pueden ser registrados en el sistema mediante sus datos personales.
2. **Creación de Pedidos**: Los pedidos pueden ser realizados indicando los platos y cantidades.
3. **Asignación de Mesas**: Los clientes pueden ser asignados a mesas disponibles.
4. **Seguimiento de Pedidos**: El personal puede actualizar y consultar el estado de los pedidos.
5. **Cálculo de Facturas**: El sistema calcula automáticamente el total de cada pedido.

## Diseño adecuado de la GUI

```markdown
### Diseño adecuado de la GUI

#### Formularios

Los formularios son esenciales para la interacción del usuario con el sistema. En Django, se pueden crear formularios utilizando `forms.py`. Aquí hay un ejemplo de cómo definir formularios para las operaciones CRUD:

```python
# forms.py
from django import forms
from .models import Cliente, Pedido, ItemPedido

class ClienteForm(forms.ModelForm):
    class Meta:
        model = Cliente
        fields = ['cedula', 'nombre', 'telefono', 'cantidad_persona']

class PedidoForm(forms.ModelForm):
    class Meta:
        model = Pedido
        fields = ['cliente', 'mesa', 'estado']

class ItemPedidoForm(forms.ModelForm):
    class Meta:
        model = ItemPedido
        fields = ['cliente', 'plato', 'cantidad', 'observacion']
```

#### Botones y Menús

Los botones y menús se definen en las plantillas HTML. Aquí hay un ejemplo de cómo crear un formulario con botones en una plantilla HTML:

```html
<!-- templates/crear_cliente.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Crear Cliente</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>
<body>
    <div class="container">
        <h2>Crear Cliente</h2>
        <form method="post">
            {% csrf_token %}
            {{ form.as_p }}
            <button type="submit" class="btn btn-primary">Guardar</button>
        </form>
    </div>
</body>
</html>
```

### Conectividad de la GUI con el backend

#### Vistas

Las vistas en `views.py` manejan las solicitudes HTTP y conectan los formularios con los modelos de la base de datos. Aquí hay un ejemplo de vistas para las operaciones CRUD:

```python
# views.py
from django.shortcuts import render, get_object_or_404, redirect
from .forms import ClienteForm, PedidoForm, ItemPedidoForm
from .models import Cliente, Pedido, ItemPedido

def crear_cliente(request):
    if request.method == 'POST':
        form = ClienteForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('lista_clientes')
    else:
        form = ClienteForm()
    return render(request, 'crear_cliente.html', {'form': form})

def lista_clientes(request):
    clientes = Cliente.objects.all()
    return render(request, 'lista_clientes.html', {'clientes': clientes})

def modificar_cliente(request, id):
    cliente = get_object_or_404(Cliente, id=id)
    if request.method == 'POST':
        form = ClienteForm(request.POST, instance=cliente)
        if form.is_valid():
            form.save()
            return redirect('lista_clientes')
    else:
        form = ClienteForm(instance=cliente)
    return render(request, 'modificar_cliente.html', {'form': form})

def eliminar_cliente(request, id):
    cliente = get_object_or_404(Cliente, id=id)
    if request.method == 'POST':
        cliente.delete()
        return redirect('lista_clientes')
    return render(request, 'eliminar_cliente.html', {'cliente': cliente})
```

### Funcionalidad: Operaciones CRUD

La interfaz debe permitir realizar las operaciones CRUD (Crear, Leer, Actualizar, Eliminar) en la base de datos. Los ejemplos anteriores muestran cómo crear, listar, modificar y eliminar clientes.

### Calidad estética y usabilidad de la interfaz

Para mejorar la apariencia y usabilidad de la interfaz, se pueden utilizar CSS y frameworks como Bootstrap. Aquí hay un ejemplo de cómo incluir Bootstrap en una plantilla HTML:

```html
<!-- templates/base.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}Mi Aplicación{% endblock %}</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>
<body>
    <div class="container">
        {% block content %}
        {% endblock %}
    </div>
</body>
</html>
```

Luego, extiende esta plantilla en otras plantillas para mantener un diseño consistente:

```html
<!-- templates/crear_cliente.html -->
{% extends 'base.html' %}

{% block title %}Crear Cliente{% endblock %}

{% block content %}
<h2>Crear Cliente</h2>
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit" class="btn btn-primary">Guardar</button>
</form>
{% endblock %}
```
```markdown
# Integración con una Base de Datos y Operaciones CRUD

## Configuración de la Base de Datos

Para este proyecto, utilizaremos SQLite como base de datos. A continuación, se detalla cómo configurar la base de datos en un proyecto Django.

1. **Instalación de Django**:
   ```bash
   pip install django
   ```

2. **Configuración de la Base de Datos en `settings.py`**:
   ```python
   # settings.py
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.sqlite3',
           'NAME': BASE_DIR / 'db.sqlite3',
       }
   }
   ```

3. **Migraciones de la Base de Datos**:
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

## Diseño adecuado de la GUI

### Formularios

Los formularios son esenciales para la interacción del usuario con el sistema. En Django, se pueden crear formularios utilizando `forms.py`. Aquí hay un ejemplo de cómo definir formularios para las operaciones CRUD:

```python
# forms.py
from django import forms
from .models import Cliente, Pedido, ItemPedido

class ClienteForm(forms.ModelForm):
    class Meta:
        model = Cliente
        fields = ['cedula', 'nombre', 'telefono', 'cantidad_persona']

class PedidoForm(forms.ModelForm):
    class Meta:
        model = Pedido
        fields = ['cliente', 'mesa', 'estado']

class ItemPedidoForm(forms.ModelForm):
    class Meta:
        model = ItemPedido
        fields = ['cliente', 'plato', 'cantidad', 'observacion']
```

### Botones y Menús

Los botones y menús se definen en las plantillas HTML. Aquí hay un ejemplo de cómo crear un formulario con botones en una plantilla HTML:

```html
<!-- templates/crear_cliente.html -->
<!DOCTYPE html>
<html>
<head>
    <title>Crear Cliente</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>
<body>
    <div class="container">
        <h2>Crear Cliente</h2>
        <form method="post">
            {% csrf_token %}
            {{ form.as_p }}
            <button type="submit" class="btn btn-primary">Guardar</button>
        </form>
    </div>
</body>
</html>
```

## Conectividad de la GUI con el backend

### Vistas

Las vistas en `views.py` manejan las solicitudes HTTP y conectan los formularios con los modelos de la base de datos. Aquí hay un ejemplo de vistas para las operaciones CRUD:

```python
# views.py
from django.shortcuts import render, get_object_or_404, redirect
from .forms import ClienteForm, PedidoForm, ItemPedidoForm
from .models import Cliente, Pedido, ItemPedido

def crear_cliente(request):
    if request.method == 'POST':
        form = ClienteForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('lista_clientes')
    else:
        form = ClienteForm()
    return render(request, 'crear_cliente.html', {'form': form})

def lista_clientes(request):
    clientes = Cliente.objects.all()
    return render(request, 'lista_clientes.html', {'clientes': clientes})

def modificar_cliente(request, id):
    cliente = get_object_or_404(Cliente, id=id)
    if request.method == 'POST':
        form = ClienteForm(request.POST, instance=cliente)
        if form.is_valid():
            form.save()
            return redirect('lista_clientes')
    else:
        form = ClienteForm(instance=cliente)
    return render(request, 'modificar_cliente.html', {'form': form})

def eliminar_cliente(request, id):
    cliente = get_object_or_404(Cliente, id=id)
    if request.method == 'POST':
        cliente.delete()
        return redirect('lista_clientes')
    return render(request, 'eliminar_cliente.html', {'cliente': cliente})
```

## Funcionalidad: Operaciones CRUD

La interfaz debe permitir realizar las operaciones CRUD (Crear, Leer, Actualizar, Eliminar) en la base de datos. Los ejemplos anteriores muestran cómo crear, listar, modificar y eliminar clientes.

## Calidad estética y usabilidad de la interfaz

Para mejorar la apariencia y usabilidad de la interfaz, se pueden utilizar CSS y frameworks como Bootstrap. Aquí hay un ejemplo de cómo incluir Bootstrap en una plantilla HTML:

```html
<!-- templates/base.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}Mi Aplicación{% endblock %}</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
</head>
<body>
    <div class="container">
        {% block content %}
        {% endblock %}
    </div>
</body>
</html>
```

Luego, extiende esta plantilla en otras plantillas para mantener un diseño consistente:

```html
<!-- templates/crear_cliente.html -->
{% extends 'base.html' %}

{% block title %}Crear Cliente{% endblock %}

{% block content %}
<h2>Crear Cliente</h2>
<form method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit" class="btn btn-primary">Guardar</button>
</form>
{% endblock %}
```

## Seguridad Básica en las Transacciones

Para asegurar las transacciones básicas, se recomienda utilizar las características de seguridad integradas en Django, como la protección CSRF (Cross-Site Request Forgery) y la validación de formularios.

## Contribución

Si deseas contribuir a este proyecto, por favor sigue los siguientes pasos:

1. Haz un fork del repositorio.
2. Crea una nueva rama (`git checkout -b feature/nueva-funcionalidad`).
3. Realiza tus cambios y haz commit (`git commit -am 'Añadir nueva funcionalidad'`).
4. Sube tus cambios a tu fork (`git push origin feature/nueva-funcionalidad`).
5. Abre un Pull Request en el repositorio original.

## Licencia

Este proyecto está licenciado bajo la Licencia MIT. Consulta el archivo `LICENSE` para más detalles.
```
