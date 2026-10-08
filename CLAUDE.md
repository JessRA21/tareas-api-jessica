# CLAUDE.md

Guía para trabajar en **tareas-api**, gestor de tareas pendientes (Generation México, CH70 Java, octubre 2026).

## Stack

- **Java 17** (configurado con `maven.compiler.release`)
- **Maven 3.8+** para build
- **JUnit 5** (Jupiter 5.10.2) para pruebas

## Comandos esenciales

```bash
# Compilar
mvn compile

# Ejecutar todas las pruebas
mvn test

# Ejecutar prueba específica
mvn test -Dtest=TareaServicioTest#crearAsignaIdConsecutivo

# Correr la demo de consola
mvn -q exec:java

# Limpiar
mvn clean
```

## Convenciones obligatorias

1. **Idioma**: Todo el código, nombres de clases, métodos, variables y comentarios en **español**.
2. **Pruebas**: JUnit 5, nombres descriptivos, **una prueba por comportamiento** (no agrupar múltiples `assert` de conceptos diferentes).
3. **Dependencias**: **No agregues dependencias nuevas al `pom.xml` sin preguntarme antes**.
4. **Sin mocks**: Las pruebas usan instancias reales de repositorio y servicio.

## Regla de oro

**Cualquier cambio que hagas debe dejar `mvn test` en verde.** Si un test falla después de tu cambio, corrígelo antes de continuar o revertir el cambio.

## Arquitectura

- **Modelo**: `Tarea`, `Prioridad`
- **Repositorio**: `TareaRepositorio` (almacenamiento en memoria, asigna IDs)
- **Servicio**: `TareaServicio` (lógica de negocio, casos de uso)
- **App**: `App.java` (demo de consola)
