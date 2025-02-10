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
# Uso de APIs e implementacion
## Aplicación de Mapa y Clima

Esta aplicación integra dos APIs externas para enriquecer la experiencia del usuario:
- **Google Maps API:** Muestra un mapa interactivo y permite la geolocalización.
- **Meteoblue API:** Proporciona un widget con información meteorológica actual y pronósticos.

## Funcionalidades

- **Mapa Interactivo:**  
  Se muestra un mapa centrado en una ubicación específica, con un marcador indicando la posición. Se utiliza la API de Google Maps para implementar esta funcionalidad.

- **Información Meteorológica:**  
  Se incorpora un widget de Meteoblue a través de un `<iframe>`, el cual muestra datos actualizados del clima.

## Implementación

### Google Maps API
![image](https://github.com/user-attachments/assets/3f947ede-44e4-419c-923c-ac3ece4772dc)

1. Se incluye el script de la API en el HTML, asegurando que se ejecute la función `initMap` al cargarse.
2. La función `initMap` crea el mapa y agrega un marcador en la ubicación deseada.
3. Es necesario reemplazar `{{ google_maps_api_key }}` por una clave válida obtenida desde la [Consola de Desarrolladores de Google Cloud](https://console.cloud.google.com/).

### Meteoblue API
![image](https://github.com/user-attachments/assets/8b2f8122-c3a0-4bcf-a65e-37f511243371)

1. Se utiliza un `<iframe>` para integrar el widget de Meteoblue.
2. La URL del widget se define en la variable `{{ meteoblue_url }}`, la cual debe configurarse correctamente para mostrar los datos meteorológicos.

## Requisitos

- Navegador web moderno con soporte para JavaScript.
- Conexión a Internet para cargar las APIs externas.

