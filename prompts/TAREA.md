# Tarea: Mi prompt avanzado

## Tarea elegida
**Generación de casos de prueba integrales para un módulo de registro de usuarios.**
El objetivo es obtener una suite de pruebas (positivas, negativas y de borde) listas para automatizar, asegurando validaciones de correo electrónico, políticas de contraseña y persistencia de datos.


## Version 1: prompt basico

```text
Escribe casos de prueba para un registro de usuarios con email y contraseña.
```
Técnica agregada: Ninguna (prompt directo/zero-shot sin restricciones).

Por qué se usó: Servir como línea base para comparar la precisión y estructura de las iteraciones posteriores.

Qué mejoró en la respuesta: La respuesta fue genérica y muy breve. Generó 3 o 4 casos triviales sin cubrir condiciones de borde, formatos específicos ni indicar la estructura esperada para automatización.

## Version 2

```text
Actúa como un QA Lead Engineer especializado en Java y JUnit 5.
Tu tarea es diseñar casos de prueba para el registro de usuarios.

Analiza los siguientes requisitos paso a paso:
1. Requisito de email: debe ser único y tener formato válido.
2. Requisito de contraseña: mínimo 8 caracteres, al menos una mayúscula, un número y un carácter especial.

Genera los casos organizados por: Casos Positivos, Casos Negativos y Casos de Borde.
Presenta cada caso de prueba con los campos: ID, Nombre del test, Datos de entrada, Resultado esperado.
```

Técnica agregada: Role Prompting (QA Lead Engineer) + Descomposición de Tarea + Prompt Estructurado.

Por qué se agregaron: Para acotar el rol técnico, forzar al modelo a categorizar las pruebas y requerir una tabla organizada.

Qué mejoró en la respuesta: La cobertura aumentó significativamente, clasificando escenarios positivos y negativos. Sin embargo, los ejemplos de código carecían de una estructura uniforme y faltaban casos extremos de concurrencia o caracteres especiales.

## Version 3: prompt final

```text
Eres un QA Automation Lead con 10 años de experiencia en pruebas de software para microservicios.

Tu tarea es diseñar y estructurar la suite completa de casos de prueba unitarios e integración para el endpoint de registro de usuarios `POST /api/v1/auth/register`.

### Requisitos del sistema:
1. Email: Obligatorio, único, máximo 100 caracteres, formato RFC 5322.
2. Contraseña: Mínimo 8 y máximo 32 caracteres; debe incluir al menos 1 mayúscula, 1 minúscula, 1 número y 1 carácter especial (`!@#$%^&*`).
3. Campos adicionales: Nombre completo (2-50 caracteres, solo letras y espacios).

### Instrucciones de razonamiento (Chain of Thought):
1. **Análisis de Requisitos:** Identifica las reglas de negocio explícitas e implícitas.
2. **Identificación de Límites:** Aplica análisis de valores límite (BVA) y partición de equivalencia.
3. **Diseño de Casos:** Clasifica los escenarios en Positivos, Negativos (Validación/Negocio) y Casos de Borde/Seguridad.

### Ejemplo de formato esperado (Few-Shot Example):
Requisito analizado: Extensión mínima de contraseña.
- ID: TC-SEC-01
- Categoría: Negativo - Validación
- Entrada: `{"email": "user@test.com", "password": "Aa1!", "fullName": "Juan Perez"}`
- Código HTTP esperado: 400 Bad Request
- Mensaje esperado: "La contraseña debe tener al menos 8 caracteres"

### Formato de Salida Obligatorio:
Responde estrictamente en formato Markdown utilizando una tabla con las columnas:
| ID | Categoría | Escenario de Prueba | Payload de Entrada (JSON) | HTTP Status Esperado | Mensaje / Condición de Salida |
```

Técnicas agregadas en v3: Combinación de Role Prompting, Chain of Thought (CoT), Few-Shot Examples y Formato Estructurado Estricto.

Por qué se agregaron: Para garantizar rigor profesional, trazabilidad del razonamiento antes de la respuesta, y asegurar que la salida sea interpretable directamente por herramientas de automatización o código.

Qué mejoró en la respuesta: La suite cubrió escenarios de inyección, caracteres Unicode, valores límite exactos y duplicidad de datos, entregando una tabla estructurada y lista para implementar sin ambigüedades.

## Tecnicas usadas en el prompt final

| Técnica | Fragmento / Sección del Prompt Final (v3) | Propósito de la Técnica |
| :--- | :--- | :--- |
| **Role Prompting** | `"Eres un QA Automation Lead con 10 años de experiencia en pruebas de software..."` | Establece la perspectiva técnica, el nivel de rigor y el vocabulario especializado sin usar la palabra "experto". |
| **Descomposición & Prompt Estructurado** | `"### Requisitos del sistema:"` y `"### Formato de Salida Obligatorio:"` | Divide el problema en especificaciones claras y delimita la estructura rígida de respuesta requerida. |
| **Chain of Thought (CoT)** | `"### Instrucciones de razonamiento (Chain of Thought): 1. Análisis... 2. Identificación de Límites..."` | Forzado de razonamiento paso a paso para analizar fronteras (BVA) y reglas de negocio antes de emitir la respuesta. |
| **Few-Shot Examples** | `"### Ejemplo de formato esperado (Few-Shot Example): ... TC-SEC-01 ..."` | Proporciona una muestra concreta de la estructura y nivel de detalle esperado para cada fila de la tabla. |

---

## Evaluacion del resultado

| Criterio de Evaluación | Cumple (Sí / No) | Observaciones |
| :--- | :---: | :--- |
| ¿El rol asignado es específico y evita la palabra genérica "experto"? | **Sí** | Se define como "QA Automation Lead con 10 años de experiencia en microservicios". |
| ¿El prompt final combina al menos tres técnicas formalmente? | **Sí** | Combina 4 técnicas: Role Prompting, Chain of Thought, Few-Shot y Prompt Estructurado. |
| ¿Se especifica un formato de respuesta claro e inequívoco? | **Sí** | Se exige estrictamente una tabla Markdown con 6 columnas y payloads en formato JSON. |
| ¿El resultado cubre validaciones de negocio, fronteras y seguridad? | **Sí** | Incluye análisis de límites (BVA), formatos RFC, duplicidad y escenarios de error/HTTP status. |

## Por que elegi estas tecnicas:

Elegí Role Prompting, Chain of Thought, Few-Shot y Prompt Estructurado porque la generación de casos de prueba requiere alta precisión lógica y estricta estandarización. Un rol técnico especializado establece la perspectiva adecuada para no olvidar la seguridad y el rendimiento. La descomposición paso a paso (Chain of Thought) garantiza que la IA analice las fronteras de datos antes de escribir resultados. Finalmente, los ejemplos Few-Shot y la definición rígida de la tabla Markdown aseguran que los artefactos generados puedan integrarse directamente en herramientas de gestión de pruebas (como Jira/Xray) sin necesidad de reescrituras manuales.