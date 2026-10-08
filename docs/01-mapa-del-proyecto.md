# Mapa del Proyecto: tareas-api

## 1. ¿Qué hace el proyecto?

Gestor de tareas pendientes en memoria que permite crear, completar, eliminar y consultar tareas con prioridades y fechas límite. Es un proyecto de práctica para el programa de mentoría técnica de Generation México (CH70 Java, octubre 2026).

## 2. Tabla de clases

| Clase | Responsabilidad | Depende de |
|-------|----------------|------------|
| `App` | Punto de entrada de la demo de consola; crea tareas de ejemplo y muestra un reporte | `TareaServicio`, `TareaRepositorio`, `Prioridad` |
| `Tarea` | Entidad de dominio que representa una tarea con título, descripción, prioridad, fecha límite y estado de completado | `Prioridad` |
| `Prioridad` | Enumeración con los tres niveles de prioridad (BAJA, MEDIA, ALTA) | - |
| `TareaRepositorio` | Almacenamiento en memoria de tareas usando `LinkedHashMap`; asigna IDs secuenciales | `Tarea` |
| `TareaServicio` | Capa de lógica de negocio con casos de uso (crear, completar, eliminar, listar, reportes) | `TareaRepositorio`, `Tarea`, `Prioridad`, `TareaNoEncontradaException` |
| `TareaNoEncontradaException` | Excepción lanzada cuando se busca una tarea por un ID inexistente | - |
| `TareaRepositorioTest` | Suite de pruebas para el repositorio (guardar, buscar, eliminar, buscar por título) | `TareaRepositorio`, `Tarea`, `Prioridad` |
| `TareaServicioTest` | Suite de pruebas para la capa de servicio (crear, completar, listar, reportes) | `TareaServicio`, `TareaRepositorio`, `Tarea`, `Prioridad` |

## 3. ¿Cómo se compila y se prueba?

```bash
# Compilar el proyecto
mvn compile

# Ejecutar todas las pruebas
mvn test

# Ejecutar una clase de prueba específica
mvn test -Dtest=TareaServicioTest

# Ejecutar un método de prueba específico
mvn test -Dtest=TareaServicioTest#crearAsignaIdConsecutivo

# Ejecutar la demo de consola
mvn -q exec:java

# Limpiar artefactos generados
mvn clean
```

## 4. Tres cosas sospechosas o incompletas

### 4.1. Cálculo invertido de días restantes
**Archivo:** `src/main/java/mx/generation/tareas/TareaServicio.java:67`

El método `diasRestantes()` calcula `ChronoUnit.DAYS.between(fechaLimite, hoy)` cuando debería ser `between(hoy, fechaLimite)`. Esto devuelve valores negativos para fechas futuras y positivos para fechas pasadas, al revés de lo que dice el comentario en la línea 59-60. El test en `TareaServicioTest.java:64` espera -3 para una tarea que vence en 3 días, confirmando que el comportamiento está invertido.

### 4.2. Filtro de prioridad excluye la prioridad mínima solicitada
**Archivo:** `src/main/java/mx/generation/tareas/TareaServicio.java:51`

El método `listarPorPrioridadMinima()` usa `>` en lugar de `>=` al comparar `ordinal()`. El comentario en línea 45-46 dice "con prioridad igual o mayor", pero el código solo devuelve tareas de prioridad **estrictamente mayor**. Por ejemplo, al pedir `Prioridad.MEDIA` no devuelve las tareas MEDIA, solo las ALTA.

### 4.3. Búsqueda por título sensible a mayúsculas a pesar del comentario
**Archivo:** `src/main/java/mx/generation/tareas/TareaRepositorio.java:44`

El método `buscarPorTitulo()` usa `contains()` sin normalizar a minúsculas, pero el comentario en línea 38-39 promete que la búsqueda es "sin distinguir mayúsculas de minúsculas". Hay un test deshabilitado en `TareaRepositorioTest.java:33-39` que documenta este fallo con `@Disabled("TODO: falla, revisar después")`.
