# interview
interview assessment


# Prueba técnica - Junior Data Science

# Prueba Técnica – Data Scientist Junior (en Español)

## 1. Contexto de Negocio

Una empresa que ofrece recetas de cocina quiere crear una herramienta para **planificación de menús** y **curación de recetas**. Desean extraer datos de un API público de recetas para:

1. **Analizar** y **organizar** la información (categorías, tiempos de cocción, dificultad, etc.).
2. **Diseñar** un proceso ETL (Extract, Transform, Load) que almacene los datos de forma adecuada.
3. **Visualizar** la información (o al menos consultar los datos de manera clara) para obtener insights que ayuden a tomar decisiones de negocio (p. ej., qué recetas incluir en un menú semanal).

---

## 2. Objetivo de la Prueba

- **Crear un pipeline** que extraiga datos de recetas desde un API.
- **Transformar** los datos (limpieza, normalización, etc.).
- **Cargar** o almacenar los datos en un formato que permita su análisis o su posterior consulta (por ejemplo, CSV/Excel o una base de datos).
- **Proponer** y **mostrar** una forma de **consultar** o **visualizar** los datos de manera sencilla (puede ser un reporte básico, una notebook con gráficos simples, una pequeña app, etc.).

En la sesión final, deberás presentar **cómo** diseñaste y construiste esta solución y explicar el **razonamiento** de cada paso.

---

## 3. Requisitos del Pipeline ETL

### 3.1 Extracción (Extract)

- Utilizar el API:
    - **Endpoint de Categorías**: `POST <https://es-mycooktouch.group-taurus.com/categories/`>
    - **Endpoint de Recetas**: `POST <https://es-mycooktouch.group-taurus.com/recipes/`>
        - Experimentar con parámetros como `limit`, `skip`, `sortBy`, etc.
    - **Detalle de Receta**: `GET <https://es-mycooktouch.group-taurus.com/recipe/><nombreReceta>`
- Mostrar cómo se realizan las llamadas a los endpoints (por ejemplo, con la librería `requests` de Python).
- Ejemplo de código:

```python
import requests
import json

response_categories = requests.post('https://es-mycooktouch.group-taurus.com/categories/')
print('\n----------------------------------')
print("This is the response of the categories:")
print(json.dumps(response_categories.json(), indent=4))

params = {
    'profile': 'desktopList',
    'limit': '2',
    'skip': '0',
    'sortBy': 'recent',
}
response_recipes = requests.post('https://es-mycooktouch.group-taurus.com/recipes/', params=params)
print('\n----------------------------------')
print("This is the response of the recipes sorted by recent:")
print(json.dumps(response_recipes.json(), indent=4))

recipe_name = response_recipes.json()['result'][0]['niceName']
print('\n----------------------------------')
recipe_url = f'https://es-mycooktouch.group-taurus.com/recipe/{recipe_name}'
recipe_response = requests.get(recipe_url)
print(f"This is the recipe name: {recipe_name}")
print(json.dumps(recipe_response.json(), indent=4))
```

### 3.2 Transformación (Transform)

- Procesar y limpiar el JSON para obtener campos relevantes (p. ej., `niceName`, `difficulty`, `cookingTime`, `ingredients`, etc.).
- Manejar datos faltantes y normalizar la estructura (tablas o DataFrames).
- Documentar en el código cada paso de la limpieza y transformación.

### 3.3 Carga (Load)

- Guardar el resultado en:
    - **Archivos CSV/Excel**, o **Base de datos** (ej.: PostgreSQL, MySQL, etc.).
- Asegurar que sea fácil consultar o extraer estos datos para la parte de visualización.

---

## 4. Consulta / Visualización de Datos

- Desarrollar una **solución sencilla** para **consultar** o **visualizar** los datos (por ejemplo, un notebook con gráficos básicos, un pequeño dashboard web, informes en PDF, etc.).
- No se requiere una herramienta específica; se valora la **creatividad** y la **eficacia** al mostrar los datos de forma clara.
- Se recomienda incluir algunos elementos o consultas como:
    - **Resumen de Categorías**: cuántas recetas hay en cada categoría.
    - **Dificultad vs. Tiempo de Cocción**: comparación entre dificultad y tiempo aproximado.
    - **Recetas Destacadas**: listado o gráfico con recetas recientes, o con mayor número de ingredientes.
    - **Filtros o consultas**: para facilitar la toma de decisiones (p. ej., ver solo recetas fáciles, con menos de X minutos de cocción, etc.).

---

## 5. Entregables

1. **Código (Python Notebooks o Scripts)**
    - Ejecución de peticiones al API (Extract).
    - Transformación de datos (limpieza, normalización, etc.).
    - Almacenamiento final (Load).
2. **Diagrama de Flujo**
    - Ilustrar la lógica ETL: API → Transformación → Almacenamiento → Consulta/Visualización.
3. **Solución de Consulta/Visualización**
    - Puede ser una notebook, un reporte, un dashboard web sencillo, etc.
    - Debe mostrar cómo se obtienen insights útiles de los datos.
4. **Presentación Final (10–15 min)**
    - Demostrar la ejecución del pipeline, cómo se procesaron los datos y cómo se consultan o visualizan.
    - Explicar decisiones tomadas y posibles mejoras futuras.
