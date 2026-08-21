# Competition Event Model

## 1. Propósito

El Competition Event Model define los eventos relevantes que ocurren dentro del dominio de competición de Tournament OS.

Un evento representa un hecho que ya ocurrió dentro del sistema.

Los eventos permiten desacoplar los procesos internos de la plataforma y establecer una base común para:

- Automatizaciones.
- Notificaciones.
- Auditoría.
- Sincronización entre módulos.
- Analytics.
- Integraciones futuras.
- Procesamiento asíncrono.
- Historial de actividad.

Un evento no representa una intención.

Por ejemplo:

```text
Crear una competición

representa una intención o comando.

Mientras que:

competition.created

representa un hecho que ya ocurrió.

2. Principios del modelo de eventos
2.1 Los eventos representan hechos

Un evento debe describir algo que ocurrió.

Los nombres deberán utilizar verbos en pasado.

Ejemplos:

competition.created
competition.published
registration.submitted
registration.approved
fixture.generated
match.finished
result.recorded

No deberán utilizarse eventos que representen órdenes o acciones futuras.

Ejemplos incorrectos:

competition.create
match.start
fixture.generate
team.register
2.2 Los eventos son inmutables

Una vez registrado un evento, su contenido no deberá modificarse.

Si posteriormente ocurre una corrección, deberá generarse un nuevo evento que represente el nuevo hecho.

Ejemplo:

result.recorded

Si el resultado requiere una corrección:

result.corrected

El evento original deberá permanecer disponible para fines de auditoría.

2.3 Los eventos no son la fuente principal de verdad

La fuente principal de verdad del sistema continuará siendo el modelo de datos transaccional.

Los eventos representan cambios o hechos ocurridos sobre esa información.

Por lo tanto:

Database State
        ↓
Domain Change
        ↓
Domain Event

El modelo de eventos no sustituye al modelo relacional.

2.4 Los eventos deben ser identificables

Cada evento deberá contar con un identificador único.

Ejemplo:

event_id

Este identificador permitirá:

Evitar procesamientos duplicados.
Mantener trazabilidad.
Relacionar eventos.
Identificar operaciones.
Auditar automatizaciones.
3. Estructura general de un evento

Todos los eventos del dominio de competición deberán seguir una estructura conceptual común.

Event
│
├── event_id
├── event_type
├── aggregate_type
├── aggregate_id
├── competition_id
├── occurred_at
├── actor_type
├── actor_id
├── correlation_id
├── causation_id
├── payload
└── metadata
4. Identificación del evento
4.1 event_id

Identificador único del evento.

Ejemplo:

evt_01JXYZ123

Características:

Único.
Inmutable.
Generado por el sistema.
Utilizado para idempotencia y auditoría.
4.2 event_type

Identifica el tipo de evento ocurrido.

Ejemplo:

competition.created

El formato general será:

aggregate.action

Ejemplos:

competition.created
competition.updated
competition.published


registration.submitted
registration.approved
registration.rejected


team.registered
team.withdrawn


fixture.generated
fixture.published


match.scheduled
match.started
match.finished


result.recorded
result.corrected
5. Aggregate

Un aggregate representa la entidad principal sobre la cual ocurrió el evento.

Cada evento deberá indicar:

aggregate_type
aggregate_id

Ejemplo:

aggregate_type: competition
aggregate_id: comp_123

Otro ejemplo:

aggregate_type: match
aggregate_id: match_456

Esto permite identificar claramente qué entidad produjo el evento.

6. Contexto de competición

Cuando un evento esté relacionado con una competición, deberá incluir:

competition_id

Ejemplo:

{
  "competition_id": "comp_123"
}

Esto permitirá:

Filtrar eventos por competición.
Ejecutar automatizaciones.
Generar auditorías.
Alimentar analytics.
Enviar notificaciones relacionadas.
7. Tiempo del evento

Cada evento deberá registrar el momento exacto en que ocurrió.

Campo:

occurred_at

Formato recomendado:

ISO 8601 UTC

Ejemplo:

2026-08-20T18:30:00Z

El tiempo del evento representa el momento del hecho dentro del sistema.

8. Actor

Un evento podrá haber sido provocado por:

Un usuario.
Un administrador.
Un árbitro.
Un sistema automatizado.
Un proceso interno.
Una integración externa.

Para identificar el origen se utilizarán:

actor_type
actor_id

Ejemplo:

{
  "actor_type": "user",
  "actor_id": "usr_123"
}

Para procesos automáticos:

{
  "actor_type": "system",
  "actor_id": "tournament-os"
}
9. Correlation ID

El campo:

correlation_id

permitirá relacionar múltiples eventos generados como consecuencia de una misma operación.

Ejemplo:

Un administrador publica una competición.

Esto podría producir:

competition.published
registration.opened
notification.requested

Todos estos eventos podrán compartir:

correlation_id: corr_123

Esto permitirá reconstruir procesos completos.

10. Causation ID

El campo:

causation_id

identificará el evento que provocó la generación del evento actual.

Ejemplo:

competition.published
        ↓
registration.opened

Entonces:

registration.opened

podrá contener:

causation_id: evt_competition_published

Esto permitirá reconstruir cadenas de eventos.

11. Payload

El payload contiene los datos específicos del evento.

Cada tipo de evento tendrá su propio payload.

Ejemplo:

{
  "event_type": "competition.created",
  "payload": {
    "competition_name": "Copa Itzaes",
    "sport": "football",
    "competition_type": "league"
  }
}

El payload deberá contener únicamente información relevante para comprender el hecho ocurrido.

No deberá utilizarse como una copia completa de todas las entidades relacionadas.

12. Metadata

Metadata contiene información técnica adicional.

Ejemplo:

{
  "metadata": {
    "schema_version": 1,
    "source": "api",
    "environment": "production"
  }
}

La metadata no deberá contener información sensible innecesaria.

13. Eventos de Competition

Los siguientes eventos representan cambios relevantes en una competición.

13.1 competition.created

Ocurre cuando una nueva competición es creada.

competition.created

Payload conceptual:

{
  "competition_name": "Copa Itzaes",
  "competition_type": "league",
  "sport": "football"
}
13.2 competition.updated

Ocurre cuando información relevante de una competición es modificada.

competition.updated

Ejemplos de modificaciones:

Nombre.
Fechas.
Configuración.
Información general.

El payload deberá indicar únicamente los datos relevantes modificados.

13.3 competition.published

Ocurre cuando una competición queda disponible oficialmente.

competition.published

Este evento podrá provocar procesos como:

Apertura de registros.
Publicación de información.
Notificaciones.
Activación de módulos.
13.4 competition.cancelled

Ocurre cuando una competición es cancelada.

competition.cancelled

El payload deberá incluir, cuando corresponda:

reason
13.5 competition.completed

Ocurre cuando una competición termina oficialmente.

competition.completed

Este evento podrá activar:

Cierre de procesos.
Generación de estadísticas.
Publicación de resultados finales.
Archivado.
14. Eventos de Registration
14.1 registration.submitted

Ocurre cuando se presenta una solicitud de inscripción.

registration.submitted
14.2 registration.approved

Ocurre cuando una inscripción es aprobada.

registration.approved
14.3 registration.rejected

Ocurre cuando una inscripción es rechazada.

registration.rejected

El payload podrá incluir:

reason
14.4 registration.cancelled

Ocurre cuando una inscripción es cancelada.

registration.cancelled
15. Eventos de Team
15.1 team.registered

Ocurre cuando un equipo queda oficialmente registrado dentro de una competición.

team.registered
15.2 team.withdrawn

Ocurre cuando un equipo abandona o es retirado de una competición.

team.withdrawn

El payload podrá incluir:

reason
16. Eventos de Fixture
16.1 fixture.generated

Ocurre cuando el sistema genera una estructura de partidos.

fixture.generated

Este evento podrá incluir información como:

{
  "fixture_id": "fixture_123",
  "match_count": 24
}
16.2 fixture.updated

Ocurre cuando el fixture es modificado.

fixture.updated
16.3 fixture.published

Ocurre cuando el fixture queda disponible oficialmente.

fixture.published

Este evento podrá activar:

Notificaciones.
Publicación pública.
Calendarios.
Sincronización con otros módulos.
17. Eventos de Match
17.1 match.created

Ocurre cuando un partido es creado.

match.created
17.2 match.scheduled

Ocurre cuando un partido recibe programación oficial.

match.scheduled

El payload podrá incluir:

Fecha.
Hora.
Instalación.
Campo.
Jornada.
17.3 match.rescheduled

Ocurre cuando un partido cambia su programación.

match.rescheduled

El payload deberá incluir información suficiente para identificar el cambio.

17.4 match.started

Ocurre cuando un partido inicia oficialmente.

match.started
17.5 match.finished

Ocurre cuando un partido termina.

match.finished

Este evento no implica necesariamente que el resultado final haya sido validado.

17.6 match.cancelled

Ocurre cuando un partido es cancelado.

match.cancelled
18. Eventos de Result
18.1 result.recorded

Ocurre cuando un resultado es registrado oficialmente.

result.recorded

Payload conceptual:

{
  "home_score": 2,
  "away_score": 1
}
18.2 result.corrected

Ocurre cuando un resultado previamente registrado es corregido.

result.corrected

El evento anterior deberá permanecer disponible para auditoría.

18.3 result.invalidated

Ocurre cuando un resultado deja de ser válido.

result.invalidated

Ejemplos:

Partido anulado.
Error administrativo.
Decisión disciplinaria.
19. Eventos de clasificación

Los cambios relevantes en las posiciones o clasificación podrán generar eventos.

19.1 standings.updated
standings.updated

Este evento indica que la clasificación fue recalculada.

No deberá utilizarse para almacenar toda la tabla de posiciones.

La tabla continuará siendo administrada por el modelo de datos correspondiente.

20. Eventos derivados

Algunos eventos podrán producir otros eventos.

Ejemplo:

result.recorded
        ↓
standings.updated
        ↓
qualification.updated

Cada evento continuará representando un hecho independiente.

21. Ejemplo completo

Un evento completo podría representarse de la siguiente manera:

{
  "event_id": "evt_01JXYZ123",
  "event_type": "result.recorded",
  "aggregate_type": "match",
  "aggregate_id": "match_456",
  "competition_id": "comp_123",
  "occurred_at": "2026-08-20T18:30:00Z",
  "actor_type": "user",
  "actor_id": "usr_789",
  "correlation_id": "corr_456",
  "causation_id": null,
  "payload": {
    "home_score": 2,
    "away_score": 1
  },
  "metadata": {
    "schema_version": 1,
    "source": "api"
  }
}
22. Procesamiento de eventos

Los consumidores de eventos deberán diseñarse para soportar procesamiento repetido.

Por lo tanto, deberán ser idempotentes.

Ejemplo:

Event received
        ↓
event_id already processed?
        ↓
Yes → Ignore safely
No
        ↓
Process event
        ↓
Mark event as processed

Esto evitará que un mismo evento genere efectos duplicados.

23. Orden de eventos

No deberá asumirse que los eventos provenientes de procesos distribuidos siempre llegarán en orden.

Cuando el orden sea relevante, deberá utilizarse información como:

occurred_at
aggregate_id
event version

El procesamiento deberá diseñarse para tolerar reintentos y entregas duplicadas.

24. Versionado del esquema

Cada evento deberá incluir una versión de esquema.

Ejemplo:

{
  "metadata": {
    "schema_version": 1
  }
}

Esto permitirá evolucionar la estructura de eventos sin romper consumidores existentes.

25. Eventos y auditoría

Los eventos podrán utilizarse como complemento del sistema de auditoría.

Permitirá responder preguntas como:

¿Qué ocurrió?
¿Cuándo ocurrió?
¿Quién lo provocó?
¿Qué entidad fue afectada?
¿Qué evento originó el cambio?

Los eventos no sustituyen necesariamente un sistema especializado de auditoría, pero proporcionan una base importante de trazabilidad.

26. Eventos y automatización

El modelo permitirá construir automatizaciones desacopladas.

Ejemplo:

fixture.published
        ↓
Notification Service
        ↓
Send notifications

Otro ejemplo:

competition.completed
        ↓
Analytics Service
        ↓
Generate competition analytics

El dominio principal no deberá depender directamente de cada automatización.

27. Eventos y notificaciones

Las notificaciones deberán reaccionar a eventos.

Ejemplo:

match.rescheduled
        ↓
notification.requested
        ↓
Notify affected participants

La modificación del partido y el envío de notificaciones deberán permanecer conceptualmente separados.

28. Eventos y analytics

Los eventos podrán alimentar sistemas analíticos.

Ejemplos:

registration.submitted

permitirá analizar:

Volumen de registros.
Horarios de mayor actividad.
Conversiones.

Mientras que:

match.finished

podrá alimentar:

Estadísticas de competición.
Actividad por jornada.
Métricas operativas.
29. Eventos y Competition Rules Engine

Los eventos deberán respetar las decisiones tomadas por el Competition Rules Engine.

Ejemplo:

result.recorded

podrá provocar:

standings.updated

Sin embargo, el cálculo de puntos y clasificación deberá respetar las reglas configuradas para la competición.

El Event Model comunica que un hecho ocurrió.

El Rules Engine determina cómo deben interpretarse determinados hechos dentro de una competición.

30. Relación con Competition State Model

El Competition State Model define los estados permitidos.

El Competition Event Model define los hechos que provocan transiciones o cambios relevantes.

Ejemplo:

Competition State


DRAFT
        │
        │ competition.published
        ↓
PUBLISHED

El evento representa el hecho ocurrido.

El State Model determina si esa transición es válida.

31. Relación con Database Constraints

Las restricciones de base de datos garantizan la integridad de la información.

Un evento nunca deberá utilizarse para evitar o sustituir una restricción crítica.

La relación conceptual será:

Command
        ↓
Validate Rules
        ↓
Validate State
        ↓
Persist Data
        ↓
Apply Constraints
        ↓
Generate Event

El evento deberá generarse únicamente después de que el cambio correspondiente haya sido aceptado por el dominio.

32. Relación con la fuente de verdad

Tournament OS mantendrá una fuente de verdad clara.

La secuencia conceptual será:

User Action
        ↓
Command
        ↓
Domain Validation
        ↓
State Validation
        ↓
Database Transaction
        ↓
Domain Event
        ↓
Consumers

Los consumidores podrán incluir:

Notificaciones.
Analytics.
Automatizaciones.
Integraciones.
Auditoría.

Los consumidores no deberán modificar directamente la lógica central de la competición sin pasar nuevamente por las reglas y validaciones correspondientes.

33. Modelo conceptual de flujo
┌───────────────┐
│ User / System │
└───────┬───────┘
        │
        ▼
┌───────────────┐
│    Command    │
└───────┬───────┘
        │
        ▼
┌───────────────────────┐
│ Competition Rules     │
│ Engine                │
└───────┬───────────────┘
        │
        ▼
┌───────────────────────┐
│ Competition State     │
│ Model                 │
└───────┬───────────────┘
        │
        ▼
┌───────────────────────┐
│ Database Transaction  │
└───────┬───────────────┘
        │
        ▼
┌───────────────────────┐
│ Domain Event          │
└───────┬───────────────┘
        │
        ├───────────────┐
        ▼               ▼
┌──────────────┐  ┌──────────────┐
│ Notifications│  │ Analytics    │
└──────────────┘  └──────────────┘
        │
        ▼
┌──────────────┐
│ Automation   │
└──────────────┘
34. Principios finales

El Competition Event Model deberá cumplir los siguientes principios:

Los eventos representan hechos ocurridos.
Los eventos son inmutables.
Cada evento posee una identidad única.
Los eventos incluyen contexto suficiente.
Los consumidores deben ser idempotentes.
No deberá asumirse orden absoluto en sistemas distribuidos.
Los eventos deberán ser versionables.
Los eventos no sustituyen la fuente principal de verdad.
Las reglas continúan siendo responsabilidad del Competition Rules Engine.
Las transiciones válidas continúan siendo responsabilidad del Competition State Model.
La integridad continúa siendo responsabilidad del modelo de datos y sus restricciones.
Los eventos permiten desacoplar automatizaciones y procesos secundarios.
35. Resultado arquitectónico

Con este modelo, Tournament OS contará con una arquitectura donde:

Competition Domain
        │
        ├── Rules
        │
        ├── State
        │
        ├── Database
        │
        └── Events
                │
                ├── Automation
                ├── Notifications
                ├── Analytics
                ├── Integrations
                └── Audit

Esto permitirá que Tournament OS evolucione desde una primera competición real, como Copa Itzaes, hacia una plataforma capaz de soportar múltiples organizaciones, competiciones y procesos sin modificar los principios fundamentales de su arquitectura.