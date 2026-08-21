# Tournament OS — Competition State Model

## 1. Propósito

Este documento define el modelo de estados y las transiciones permitidas para una competencia dentro de Tournament OS.

El modelo establece el ciclo de vida formal de una competencia y evita que los módulos del sistema manejen estados de forma independiente o contradictoria.

Toda modificación del estado de una competencia deberá respetar las reglas definidas en este documento.

---

# 2. Principio general

Una competencia deberá tener siempre un único estado operativo vigente.

El estado representa la situación actual de la competencia dentro de su ciclo de vida.

Los módulos que necesiten conocer la situación de una competencia deberán consultar el estado central de la competencia y no mantener una copia independiente del mismo.

---

# 3. Estados oficiales

Tournament OS utilizará los siguientes estados principales:

| Estado | Identificador | Descripción |
|---|---|---|
| Borrador | `DRAFT` | La competencia está siendo configurada y todavía no ha sido publicada. |
| Publicada | `PUBLISHED` | La competencia ha sido publicada, pero todavía no se encuentra abierta para registros. |
| Registro abierto | `REGISTRATION_OPEN` | Los equipos o participantes pueden realizar su registro. |
| Registro cerrado | `REGISTRATION_CLOSED` | El periodo de registro ha terminado. |
| En preparación | `READY` | La competencia está configurada y preparada para comenzar. |
| En curso | `IN_PROGRESS` | La competencia se encuentra desarrollándose. |
| Finalizada | `FINISHED` | La competencia ha concluido deportivamente. |
| Archivada | `ARCHIVED` | La competencia ha sido cerrada operativamente y conservada como histórico. |
| Cancelada | `CANCELLED` | La competencia fue cancelada y no continuará su ciclo normal. |

---

# 4. Estado DRAFT

## Identificador

`DRAFT`

## Descripción

Representa una competencia que todavía se encuentra en proceso de configuración.

En este estado se pueden definir o modificar elementos como:

- Nombre.
- Temporada.
- Tipo de competencia.
- Categorías.
- Fechas.
- Sedes.
- Reglas.
- Formato.
- Número de participantes.
- Parámetros de inscripción.
- Configuración deportiva.
- Configuración administrativa.

## Restricciones

Una competencia en `DRAFT`:

- No deberá aceptar registros públicos.
- No deberá generar jornadas oficiales.
- No deberá generar resultados oficiales.
- No deberá aparecer como competencia activa para los participantes.

---

# 5. Estado PUBLISHED

## Identificador

`PUBLISHED`

## Descripción

La competencia ha sido configurada y publicada oficialmente dentro de Tournament OS.

La publicación significa que la información básica de la competencia está disponible para consulta.

## Restricciones

Una competencia en `PUBLISHED`:

- Todavía no acepta registros.
- No deberá iniciar encuentros.
- No deberá generar resultados deportivos oficiales.

---

# 6. Estado REGISTRATION_OPEN

## Identificador

`REGISTRATION_OPEN`

## Descripción

Representa el periodo durante el cual los equipos, jugadores o participantes autorizados pueden registrarse en la competencia.

## Operaciones permitidas

Durante este estado podrán ejecutarse procesos como:

- Registro de equipos.
- Registro de participantes.
- Validación de documentación.
- Revisión de requisitos.
- Gestión de solicitudes.
- Confirmación de participantes.

## Restricciones

Una competencia en `REGISTRATION_OPEN`:

- No deberá iniciar encuentros oficiales.
- No deberá registrar resultados deportivos oficiales.
- No deberá considerarse todavía en fase competitiva.

---

# 7. Estado REGISTRATION_CLOSED

## Identificador

`REGISTRATION_CLOSED`

## Descripción

Indica que el periodo de registro ha terminado.

A partir de este estado, la competencia deja de aceptar nuevos registros ordinarios.

## Operaciones permitidas

Podrán realizarse procesos administrativos como:

- Validación final de participantes.
- Revisión de documentación.
- Confirmación de equipos.
- Corrección de información autorizada.
- Preparación del calendario competitivo.

---

# 8. Estado READY

## Identificador

`READY`

## Descripción

Representa una competencia completamente configurada y preparada para comenzar.

En este estado deberán encontrarse disponibles los elementos necesarios para iniciar la operación competitiva.

Entre ellos pueden encontrarse:

- Participantes confirmados.
- Formato competitivo definido.
- Calendario generado.
- Jornadas configuradas.
- Sedes asignadas cuando corresponda.
- Reglas activas.
- Estructura competitiva validada.

## Restricciones

Una competencia `READY` todavía no está oficialmente en curso.

---

# 9. Estado IN_PROGRESS

## Identificador

`IN_PROGRESS`

## Descripción

Representa una competencia que se encuentra actualmente en desarrollo.

Durante este estado pueden producirse eventos deportivos oficiales.

Entre ellos:

- Partidos.
- Resultados.
- Jornadas.
- Clasificaciones.
- Estadísticas.
- Incidencias.
- Suspensiones.
- Reprogramaciones.
- Actualizaciones de posiciones.

Los datos generados durante este estado deberán respetar las reglas de competencia y la fuente única de verdad definida por la arquitectura.

---

# 10. Estado FINISHED

## Identificador

`FINISHED`

## Descripción

Representa una competencia que ha concluido deportivamente.

En este estado deberán encontrarse determinados los resultados finales de la competencia.

Podrán consolidarse:

- Clasificación final.
- Resultados finales.
- Campeón.
- Subcampeón.
- Estadísticas finales.
- Reconocimientos.
- Historial competitivo.

## Restricciones

Una competencia `FINISHED` no deberá volver automáticamente a `IN_PROGRESS`.

Cualquier corrección posterior deberá realizarse mediante mecanismos formales de corrección o auditoría.

---

# 11. Estado ARCHIVED

## Identificador

`ARCHIVED`

## Descripción

Representa una competencia que ha sido cerrada operativamente y conservada como histórico.

El archivado no implica eliminación de información.

Los datos históricos deberán permanecer disponibles para consulta y análisis cuando corresponda.

## Restricciones

Una competencia archivada:

- No deberá aceptar registros.
- No deberá generar nuevos encuentros.
- No deberá modificar resultados de forma ordinaria.
- No deberá regresar a estados operativos normales.

---

# 12. Estado CANCELLED

## Identificador

`CANCELLED`

## Descripción

Representa una competencia que ha sido cancelada antes de completar su ciclo normal.

La cancelación deberá conservarse como parte del historial de la competencia.

No deberá utilizarse como mecanismo para eliminar una competencia.

## Información de auditoría

Cuando una competencia sea cancelada deberá conservarse, cuando corresponda:

- Fecha de cancelación.
- Usuario que realizó la acción.
- Motivo.
- Estado anterior.
- Información relevante de la operación.

---

# 13. Transiciones oficiales

Las transiciones permitidas serán las siguientes:

```text
DRAFT
  ↓
PUBLISHED
  ↓
REGISTRATION_OPEN
  ↓
REGISTRATION_CLOSED
  ↓
READY
  ↓
IN_PROGRESS
  ↓
FINISHED
  ↓
ARCHIVED

La disponibilidad de la cancelación deberá estar condicionada por las reglas administrativas y deportivas de la competencia.

14. Matriz de transiciones
Estado actual	Estado siguiente	Permitido
DRAFT	PUBLISHED	Sí
DRAFT	CANCELLED	Sí
PUBLISHED	REGISTRATION_OPEN	Sí
PUBLISHED	CANCELLED	Sí
REGISTRATION_OPEN	REGISTRATION_CLOSED	Sí
REGISTRATION_OPEN	CANCELLED	Sí
REGISTRATION_CLOSED	READY	Sí
REGISTRATION_CLOSED	CANCELLED	Sí
READY	IN_PROGRESS	Sí
READY	CANCELLED	Sí
IN_PROGRESS	FINISHED	Sí
IN_PROGRESS	CANCELLED	Sí
FINISHED	ARCHIVED	Sí
ARCHIVED	Cualquier estado operativo	No
CANCELLED	Cualquier estado operativo	No
15. Transiciones no permitidas

No deberán permitirse transiciones arbitrarias entre estados.

Ejemplos:

DRAFT → IN_PROGRESS
DRAFT → FINISHED
PUBLISHED → IN_PROGRESS
REGISTRATION_OPEN → IN_PROGRESS
REGISTRATION_CLOSED → IN_PROGRESS
FINISHED → IN_PROGRESS
ARCHIVED → IN_PROGRESS
CANCELLED → IN_PROGRESS

Estas transiciones deberán rechazarse por la lógica de dominio.

16. Reglas de integridad

Toda transición deberá cumplir las condiciones requeridas por el estado destino.

Por ejemplo:

Antes de pasar de:

REGISTRATION_CLOSED → READY

deberá verificarse que la competencia cuenta con la configuración mínima necesaria para comenzar.

Antes de pasar de:

READY → IN_PROGRESS

deberá verificarse que la competencia está preparada para iniciar su operación deportiva.

Antes de pasar de:

IN_PROGRESS → FINISHED

deberá verificarse que la competencia puede considerarse deportivamente concluida.

17. Fuente única de verdad

El estado de la competencia deberá almacenarse en una única entidad central de competencia.

Los módulos relacionados no deberán mantener estados independientes que puedan entrar en conflicto.

Ejemplo:

Incorrecto:

Competition.status
Calendar.status
Registration.status
Tournament.status

cuando cada uno pretende representar el estado general de la competencia.

Correcto:

Competition.status

como estado central.

Los demás módulos podrán mantener estados propios únicamente cuando representen conceptos diferentes.

18. Historial de transiciones

Toda transición relevante deberá poder ser auditada.

El sistema deberá conservar, cuando corresponda:

Estado anterior.
Estado nuevo.
Fecha y hora.
Usuario o proceso que ejecutó la transición.
Motivo.
Metadatos relevantes.

Ejemplo conceptual:

DRAFT
    ↓
PUBLISHED


Actor: usuario
Fecha: YYYY-MM-DD HH:MM:SS
Motivo: competencia publicada
19. Reglas para Tournament OS

El motor de competencia deberá ser responsable de validar las transiciones.

Los módulos de interfaz no deberán modificar directamente el estado sin pasar por las reglas de dominio correspondientes.

Conceptualmente:

Usuario
   ↓
Interfaz
   ↓
Acción de dominio
   ↓
Validación de transición
   ↓
Cambio de estado
   ↓
Persistencia
   ↓
Historial / auditoría
20. Relación con Competition Rules Engine

El modelo de estados no reemplaza al motor de reglas de competencia.

Ambos componentes tienen responsabilidades diferentes.

Competition State Model

Define:

En qué estado se encuentra la competencia.
Qué estados existen.
Qué transiciones son válidas.
Qué transiciones están prohibidas.
Competition Rules Engine

Define:

Cómo funciona deportivamente la competencia.
Cómo se calculan resultados.
Cómo se determina la clasificación.
Cómo se aplican reglas específicas.
Cómo se procesan excepciones deportivas.

La relación conceptual es:

Competition State Model
          +
Competition Rules Engine
          ↓
Competition Domain
21. Relación con el modelo relacional

El estado de la competencia deberá poder persistirse dentro del modelo de datos relacional definido por Tournament OS.

El modelo físico deberá respetar:

Identificador estable.
Estado válido.
Integridad referencial.
Restricciones de dominio.
Auditoría cuando corresponda.

La implementación concreta de tablas, columnas, índices y restricciones deberá mantenerse alineada con:

database/relational-model.md
database/database-constraints.md
22. Relación con el Competition Domain Model

El modelo de estados forma parte del dominio de Competition.

Por lo tanto:

Competition
 ├── Identity
 ├── Configuration
 ├── Participants
 ├── Rules
 ├── Schedule
 ├── State
 └── Audit

El estado deberá ser tratado como una propiedad de dominio y no únicamente como un valor visual de la interfaz.

23. Principios de diseño

Este modelo deberá respetar la filosofía general de Tournament OS:

Una sola fuente de verdad

El estado de la competencia deberá existir de forma centralizada.

Automatización

Las transiciones podrán ejecutarse automáticamente cuando se cumplan las condiciones previamente definidas.

Inteligencia

El sistema podrá utilizar el estado para generar recomendaciones y acciones apropiadas.

Simplicidad

La complejidad de las transiciones deberá permanecer dentro del dominio y no trasladarse innecesariamente al usuario.

Escalabilidad

El modelo deberá funcionar para:

Academias.
Clubes amateurs.
Clubes profesionales.
Universidades.
Ligas.
Asociaciones.
Federaciones.

Sin requerir una modificación estructural del modelo principal.

24. Principio de extensibilidad

Tournament OS podrá incorporar estados adicionales en el futuro cuando exista una necesidad de dominio claramente justificada.

La incorporación de un nuevo estado deberá:

Tener una definición formal.
Tener un identificador estable.
Definir sus condiciones de entrada.
Definir sus condiciones de salida.
Definir sus transiciones permitidas.
Definir sus restricciones.
Mantener compatibilidad con el modelo de dominio.
Mantener integridad con el modelo relacional.

No deberán agregarse estados únicamente para resolver necesidades temporales de interfaz.

25. Estado como máquina de estados

Conceptualmente, la competencia puede representarse como una máquina de estados finitos:

                 ┌──────────────┐
                 │    DRAFT     │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │  PUBLISHED   │
                 └──────┬───────┘
                        │
                        ▼
             ┌─────────────────────┐
             │ REGISTRATION_OPEN   │
             └──────────┬──────────┘
                        │
                        ▼
             ┌─────────────────────┐
             │ REGISTRATION_CLOSED │
             └──────────┬──────────┘
                        │
                        ▼
                 ┌──────────────┐
                 │    READY     │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │ IN_PROGRESS  │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │   FINISHED   │
                 └──────┬───────┘
                        │
                        ▼
                 ┌──────────────┐
                 │   ARCHIVED   │
                 └──────────────┘




Estados cancelables:


DRAFT ────────────────┐
PUBLISHED ────────────┤
REGISTRATION_OPEN ────┤
REGISTRATION_CLOSED ──┤──→ CANCELLED
READY ────────────────┤
IN_PROGRESS ──────────┘
26. Regla final

Ningún módulo deberá asumir que una competencia puede realizar una acción únicamente porque dicha acción existe en la interfaz.

Toda operación deberá considerar:

Estado actual
      +
Reglas de dominio
      +
Condiciones de la operación
      ↓
¿Transición válida?
      ↓
Sí → ejecutar
No → rechazar