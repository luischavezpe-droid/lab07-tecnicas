# Bitacora de tecnicas avanzadas
Laboratorio 07: Tecnicas Avanzadas de Prompting.
Herramienta de IA usada: (escribe aqui cual usaste)
## Ejercicio 2: Zero-shot, one-shot y few-shot
| Tipo | Aciertos (de 5) | Formato de la respuesta |(Si/No) |
|------|-----------------|-------------------------|--------------------------------
----|
| Zero-shot |5 |Enumerado, separador por una linea, Titulos y clasificacion |si|
| One-shot | 5| Enumerado, entre comillas y con flechas|Si |
| Few-shot | 5|Solo entre comillas y con flehcas |Si |


## Ejercicio 3: Chain of Thought
| Pedido | Respuesta de la IA | Muestra los pasos (Si/No) | Correcta (Si/No) |
|--------|--------------------|---------------------------|------------------|
| Directo |318.60 |No |Si |
| Paso a paso |Me dio la explicacion paso a paso y me dijo como resolverlo, y al final me dio la respuesta |Si |Si |

## Ejercicio 4: Role prompting
| Version | Vocabulario (sencillo/tecnico) | Usa ejemplos o codigo | A quien le sirve mas |
|---------|-------------------------------|-----------------------|----------------------|
| A. Sin rol |Sencillo |Si |Principiante |
| B. Rol docente |Sencillo |Si |Intermedio |
| C. Rol senior |Tecnico |Si |Intermedio |
## Ejercicio 5: Descomposicion
| Paso | Entregable de la IA | Comparación vs. Pedido |
| :--- | :--- | :--- |
| **1. Requisitos** | Lista de 5 módulos principales del sistema de inventario. | Cumplió exactamente con la cantidad y contexto solicitados. |
| **2. Clases** | 6 clases estructuradas con atributos y tipos de datos. | Tradujo fielmente los 5 requisitos a un diseño orientado a objetos. |
| **3. Código** | Código Java de la clase `Producto` con constructor y getters/setters. | Entregó la estructura exacta solicitada sin faltar nada. |
| **4. Mejoras** | 3 optimizaciones (`BigDecimal`, métodos de dominio y validaciones). | Aportó exactamente las 3 mejoras concretas requeridas. |
## Ejercicio 6: Prompt estructurado y autocritica
| Qué revisar | Cumple (Sí / No) |
| :--- | :--- |
| ¿Tiene las 4 columnas pedidas? |Si |
| ¿Incluye el bloqueo después de 3 intentos? |Si |
| ¿Incluye casos con campos vacíos? |Si |
| ¿Indica qué casos agregó en la autocrítica? |Si |
| ¿Hay algún caso repetido o que no tenga sentido? |No   |

```text
PROMPT:
<rol>Actua como analista de pruebas de software.</rol>
<contexto>Login web con correo y contrasena. La cuenta se bloquea despues de 3 intentos fallidos.</contexto>
<tarea>Piensa paso a paso que puede fallar y escribe 6 casos de prueba.</tarea>
<formato>Tabla con las columnas: ID, escenario, datos de entrada, resultado esperado.</formato>

CRITICA:
Totalmente, me toca asumir el bug en esta.

En la primera respuesta me ceñí de forma tan rígida al límite de los 6 casos de prueba que pediste originalmente que prioricé la lógica principal del bloqueo (los 3 intentos, el reseteo del contador y la denegación) y empaqueté las validaciones de entrada en un solo caso genérico. Al hacer eso, dejé fuera escenarios de borde fundamentales como los espacios en blanco o la falta sintáctica del @.

Un buen analista de QA debe anticiparse a los casos límite desde el día uno y no esperar a que el Product Owner le haga el code review. Tomo nota para no escatimar en la cobertura inicial.
```
