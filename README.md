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

### Resumen

- **Formularios**: Definidos en `forms.py` para las operaciones CRUD.
- **Botones y Menús**: Definidos en las plantillas HTML.
- **Conectividad con el Backend**: Vistas en `views.py` que manejan las solicitudes HTTP y conectan los formularios con los modelos.
- **Calidad Estética y Usabilidad**: Uso de CSS y frameworks como Bootstrap para mejorar la apariencia y usabilidad de la interfaz.
