# Conceptos

## Rule Library

La Rule Library es el repositorio central donde una organización administra todos sus reglamentos (Rule Sets).

Su función es almacenar, versionar y reutilizar los reglamentos que podrán ser utilizados por una o múltiples competiciones.

Características:

- Una organización posee una única Rule Library.
- Un Rule Set puede reutilizarse en múltiples competiciones.
- Todos los cambios conservan historial.
- Ningún Rule Set publicado puede modificarse directamente.
- Cada nueva modificación genera una nueva versión o una copia del reglamento.

---

## Rule Set

El Rule Set representa un reglamento completo y versionable que contiene el conjunto organizado de Rule Categories y Rules que definen un reglamento deportivo.

El Rule Set funciona como el contenedor lógico del reglamento.

Un Rule Set puede tener múltiples Rule Versions. Cada Rule Version representa una configuración específica e inmutable del Rule Set.

La separación entre Rule Set y Rule Version permite mantener un reglamento como una entidad reutilizable y, al mismo tiempo, conservar versiones históricas independientes.

Cada Rule Set pertenece únicamente a una Rule Library.

Un Rule Set puede:

- Crearse desde cero.
- Clonarse a partir de otro reglamento.
- Contener múltiples Rule Categories.
- Contener múltiples Rules.
- Generar nuevas Rule Versions.
- Publicar nuevas Rule Versions.
- Archivar el reglamento cuando deje de utilizarse.
- Mantener historial completo de versiones.
- Ser utilizado como base para diferentes competiciones.

El Rule Set no representa por sí mismo una configuración histórica inmutable.

La configuración histórica e inmutable está representada por cada Rule Version.

Ejemplo:

Reglamento Copa Itzaes

├── Rule Version 1.0
├── Rule Version 1.1
├── Rule Version 2.0
└── Rule Version 2.1

Cada una de estas Rule Versions permanece inmutable después de su publicación.

Si el organizador necesita realizar cambios al reglamento, deberá generar una nueva Rule Version.

La versión anterior permanecerá intacta y podrá continuar siendo utilizada por las competiciones que fueron asignadas a ella.

Información principal de un Rule Set:

| Campo | Descripción |
|--------|-------------|
| Rule Set ID | Identificador único |
| Nombre | Nombre del reglamento |
| Descripción | Objetivo del reglamento |
| Organización | Organización propietaria |
| Estado | Draft, Validating, Published, Archived |
| Fecha de creación | Registro histórico |
| Fecha de actualización | Última modificación del contenedor |
| Total de categorías | Número de Rule Categories |
| Total de reglas | Número total de Rules |
| Total de versiones | Número de Rule Versions |
| Versión publicada actual | Última Rule Version publicada |

El Rule Set constituye el contenedor lógico oficial de un reglamento dentro de Tournament OS™.

La Rule Version constituye la unidad histórica e inmutable que podrá ser asignada a una Competition Category.

---

## Rule Category

Una Rule Category agrupa reglas relacionadas con un mismo tema.

Su objetivo es facilitar la administración del reglamento.

Ejemplos:

- Registro
- Competencia
- Puntuación
- Desempates
- Playoffs
- Sanciones
- Protestas
- Credenciales
- Arbitraje
- Finanzas
- Notificaciones

---

## Rule

Una Rule representa la unidad mínima de configuración del Competition Rules Engine.

Cada Rule define un único comportamiento o decisión de negocio que el sistema deberá poder evaluar y ejecutar sin modificar el código fuente de Tournament OS™.

Una Rule no contiene otras Rules.

Las Rules pueden reutilizarse en diferentes Rule Sets. Su definición representa el comportamiento que controla el sistema, mientras que los valores y configuraciones utilizados por una competencia quedan determinados por la Rule Version correspondiente.

Esta independencia es de identidad y definición, no de comportamiento en ejecución: una Rule puede declarar relaciones con otras Rules mediante Rule Dependencies o Rule Conditions sin dejar de ser una entidad independiente y reutilizable.

Cada Rule debe tener un único propósito y no deberá controlar más de una decisión independiente del reglamento.

Ejemplos:

- Máximo de jugadores registrados.
- Puntos por victoria.
- Permitir tiempos extra.
- Permitir serie de penales.
- Edad mínima permitida.
- Número máximo de extranjeros.
- Cantidad de sustituciones.
- Duración del partido.
- Acumulación de tarjetas amarillas.
- Sistema de desempate.

La Rule define qué comportamiento controla el sistema.

Los Rule Parameters definen los valores configurables de ese comportamiento.

La Rule Version determina la configuración exacta que deberá utilizar una competencia.

Las modificaciones de una Rule utilizada dentro de una Rule Version publicada no deberán alterar dicha versión histórica. Cualquier modificación deberá realizarse mediante una nueva configuración o nueva Rule Version según corresponda.

Información principal de una Rule:

| Campo | Descripción |
|--------|-------------|
| Rule ID | Identificador único |
| Nombre | Nombre corto de la regla |
| Categoría | Rule Category propietaria |
| Scope | Nivel donde aplica la regla |
| Tipo | Integer, Boolean, Decimal, Texto, Fecha, Lista, etc. |
| Valor por defecto | Valor inicial del sistema |
| Configurable | Sí / No |
| Heredable | Sí / No |
| Visible | Sí / No |
| Requerida | Sí / No |
| Dependencias | Reglas relacionadas |
| Validaciones | Restricciones permitidas |
| Fecha de creación | Fecha de registro |
| Estado | Draft, En revisión, Publicada, Activa, Obsoleta, Archivada |
| Descripción | Explica el propósito de la regla |

Una Rule puede ser utilizada como componente dentro de diferentes Rule Sets sin perder su identidad.

La configuración efectiva utilizada por una competencia será determinada por la Rule Version asignada y, cuando corresponda, por los Rule Overrides permitidos para la Competition Category.

---

## Tipos de reglas

Las reglas pueden administrar diferentes tipos de información.

- Integer
- Decimal
- Boolean
- Texto
- Fecha
- Hora
- Lista
- Enumeración
- Moneda
- Porcentaje
- Objeto
- Colección

El tipo de dato determinará la forma en que el sistema validará y presentará la configuración al organizador.

---

## Rule Parameter

Una Rule puede estar compuesta por uno o varios parámetros configurables.

Estos parámetros permiten modificar el comportamiento de una regla sin alterar su definición.

Ejemplo

Regla

Máximo de jugadores registrados

Parámetros

Valor actual: 28

Valor mínimo: 1

Valor máximo: 40

Editable: Sí

Visible: Sí

Obligatorio: Sí

---

# Ciclo de Vida

El Competition Rules Engine administra el ciclo de vida tanto de las reglas individuales (Rules) como de los reglamentos completos (Rule Sets).

El objetivo es garantizar la integridad histórica de las competiciones y evitar que un cambio en un reglamento afecte torneos que ya se encuentran en operación.

---

## Ciclo de Vida de una Regla (Rule)

Toda Rule seguirá el siguiente flujo de estados.

Draft

↓

En revisión

↓

Publicada

↓

Activa

↓

Obsoleta

↓

Archivada

### Draft

La regla está siendo creada y aún no puede utilizarse en un reglamento publicado.

### En revisión

La regla está siendo validada por la organización antes de su publicación.

### Publicada

La regla ya puede incorporarse a nuevos Rule Sets.

### Activa

La regla está siendo utilizada por uno o varios reglamentos publicados.

### Obsoleta

La regla ya no será utilizada en nuevos reglamentos, pero continúa existiendo para conservar el historial de competiciones anteriores.

### Archivada

La regla queda únicamente como registro histórico y no podrá reutilizarse.

---

## Ciclo de Vida de un Rule Set

El Rule Set en sí mismo tiene estados persistentes: Draft, Validating, Published, Archived.

"Asignado a competiciones" y "Versionado" no son estados del Rule Set — son eventos que ocurren sobre sus Rule Versions. Se documentan aquí porque forman parte del flujo funcional completo del reglamento.

Borrador

↓

Configuración

↓

Validación

↓

Publicado

↓

Asignado a competiciones (evento sobre una Rule Version, no un estado del Rule Set)

↓

Versionado (creación de una nueva Rule Version)

↓

Archivado

### Borrador

El organizador comienza la creación del reglamento. Estado del Rule Set: Draft.

### Configuración

Se agregan categorías, reglas y parámetros. Estado del Rule Set: Draft.

### Validación

El sistema verifica que todas las reglas obligatorias existan y que no haya configuraciones incompatibles. Estado del Rule Set: Validating.

### Publicado

Se genera una Rule Version publicada, disponible para utilizarse en nuevas competiciones. Estado del Rule Set: Published.

### Asignado

Una o varias Competition Categories han creado un Rule Assignment hacia una Rule Version de este Rule Set.

Esto no es un estado del Rule Set — es la existencia de uno o varios Rule Assignments activos. Una vez que una Competition Category inicia su participación oficial, su Rule Assignment queda bloqueado (ver Rule Assignment).

### Versionado

Si el organizador necesita realizar cambios, el sistema creará automáticamente una nueva Rule Version.

La Rule Version anterior permanecerá intacta y seguirá siendo utilizada por las categorías que fueron asignadas a ella.

### Archivado

El Rule Set deja de estar disponible para nuevas competiciones, pero conserva toda la información histórica. Estado del Rule Set: Archived.
---

# Arquitectura del Competition Rules Engine

El Competition Rules Engine está compuesto por un conjunto de entidades especializadas que trabajan de forma coordinada para administrar los reglamentos deportivos de Tournament OS™.

Cada entidad tiene una única responsabilidad y puede evolucionar de forma independiente, permitiendo que el sistema sea escalable, reutilizable y completamente configurable.

La arquitectura del motor se basa en la siguiente jerarquía:

Organization
    │
    └── Rule Library
            │
            └── Rule Set
                    │
                    ├── Rule Category
                    │       ├── Rule
                    │       │      └── Rule Parameter
                    │
                    ├── Rule Version
                    │
                    ├── Rule Assignment
                    │
                    └── Rule Override

Esta arquitectura garantiza la reutilización de reglamentos, la trazabilidad histórica y la configuración flexible de cualquier competencia sin modificar el código fuente.

---

## Organization

La Organization representa a la institución propietaria de la información.

Cada organización administra su propia Rule Library de manera independiente.

Ejemplos:

- Copa Itzaes
- Liga MX
- Federación Mexicana de Fútbol
- CONCACAF
- FIFA

Una organización nunca comparte directamente sus reglamentos con otra organización, salvo mediante procesos de exportación o importación.

---

## Rule Library

Cada Organization posee una única Rule Library.

La Rule Library funciona como el repositorio central de todos los Rule Sets creados por la organización.

Desde este repositorio el organizador podrá:

- Crear nuevos reglamentos.
- Clonar reglamentos existentes.
- Versionar reglamentos.
- Publicar reglamentos.
- Archivar reglamentos.
- Buscar reglamentos.
- Reutilizar reglamentos en distintas competiciones.

La Rule Library constituye la única fuente oficial de reglamentos de la organización.
---

## Rule Set

El Rule Set representa un reglamento completo listo para ser utilizado por una o varias competiciones.

Está formado por un conjunto organizado de Rule Categories y Rules que describen completamente el comportamiento deportivo, administrativo y disciplinario de una competencia.

Cada Rule Set representa un reglamento lógico que puede contar con múltiples Rule Versions a lo largo del tiempo.

Cada Rule Version constituye una representación específica e inmutable de ese reglamento en un momento determinado, garantizando que las competiciones que la utilizan nunca cambien su comportamiento por modificaciones posteriores.

Un Rule Set puede:

- Crearse desde cero.
- Clonarse a partir de otro reglamento.
- Versionarse.
- Publicarse.
- Archivarse.
- Asignarse a múltiples competiciones.
- Mantener historial completo de cambios.

Cada Rule Set pertenece únicamente a una Rule Library.

Una Rule Version publicada es inmutable.

El Rule Set continúa existiendo como contenedor lógico del reglamento y puede generar nuevas Rule Versions.

Cuando el organizador necesita realizar cambios sobre un reglamento publicado, Tournament OS™ deberá crear una nueva Rule Version.

La Rule Version anterior permanecerá intacta y continuará disponible para las competiciones que hayan sido asignadas a ella.

Información principal de un Rule Set:

| Campo | Descripción |
|--------|-------------|
| Rule Set ID | Identificador único |
| Nombre | Nombre del reglamento |
| Descripción | Objetivo del reglamento |
| Organización | Propietario del reglamento |
| Estado | Draft, Validating, Published, Archived |
| Fecha de creación | Registro histórico |
| Fecha de actualización | Última modificación del contenedor |
| Basado en | Rule Set origen (si fue clonado) |
| Total de categorías | Número de Rule Categories |
| Total de reglas | Número total de Rules |
| Total de versiones | Número de Rule Versions |
| Versión publicada actual | Última Rule Version publicada |
| Competiciones asociadas | Cantidad de competiciones que lo utilizan |

Un Rule Set constituye la unidad oficial de reglamentación dentro de Tournament OS™.
---

## Rule Category

Una Rule Category representa una agrupación lógica de Rules relacionadas entre sí.

Su objetivo es organizar el reglamento en áreas funcionales para facilitar su administración, mantenimiento y reutilización.

Cada Rule Category pertenece exclusivamente a un Rule Set.

Una categoría no contiene lógica propia; únicamente agrupa reglas que comparten un mismo propósito.

Ejemplos de Rule Categories:

- Registro de jugadores
- Registro de entrenadores
- Registro de cuerpos técnicos
- Competencia
- Calendario
- Partidos
- Arbitraje
- Sanciones
- Protestas
- Estadísticas
- Premiación
- Finanzas
- Credenciales
- Notificaciones
- Playoffs
- Desempates

Una Rule Category puede contener desde una sola Rule hasta cientos de Rules.

Información principal de una Rule Category:

| Campo | Descripción |
|--------|-------------|
| Rule Category ID | Identificador único |
| Nombre | Nombre de la categoría |
| Descripción | Propósito de la categoría |
| Orden | Posición dentro del reglamento |
| Total de Rules | Número de reglas contenidas |
| Estado | Activa / Archivada |

Las Rule Categories permiten dividir reglamentos complejos en módulos independientes, facilitando su navegación y mantenimiento.
---

## Rule

La Rule representa la unidad mínima de configuración del Competition Rules Engine.

Cada Rule controla un único comportamiento del sistema.

Una Rule nunca contiene otras Rules.

Toda la inteligencia del reglamento está formada por miles de Rules organizadas dentro de diferentes Rule Categories.

Cada Rule es completamente independiente en su identidad y definición: puede crearse, versionarse y reutilizarse en múltiples Rule Sets sin depender de que otra Rule exista. Esto no excluye que, en tiempo de ejecución, una Rule declare relaciones con otras Rules mediante Rule Dependencies o Rule Conditions.

Una Rule únicamente describe una decisión de negocio.

Ejemplos:

- Máximo de jugadores registrados.
- Puntos por victoria.
- Permitir tiempos extra.
- Permitir Shoot Out.
- Edad mínima permitida.
- Número de extranjeros.
- Cantidad de sustituciones.
- Duración del partido.
- Acumulación de tarjetas amarillas.
- Sistema de desempate.

Cada Rule posee un único propósito.

Nunca deberá controlar más de una decisión del reglamento.

Información principal de una Rule:

| Campo | Descripción |
|--------|-------------|
| Rule ID | Identificador único |
| Nombre | Nombre corto de la regla |
| Categoría | Rule Category propietaria |
| Scope | Nivel donde aplica la regla |
| Tipo | Integer, Boolean, Decimal, Texto, Fecha, Lista, etc. |
| Valor por defecto | Valor inicial del sistema |
| Configurable | Sí / No |
| Heredable | Sí / No |
| Visible | Sí / No |
| Requerida | Sí / No |
| Dependencias | Reglas relacionadas |
| Validaciones | Restricciones permitidas |
| Fecha de creación | Fecha de registro |
| Estado | Draft, En revisión, Publicada, Activa, Obsoleta, Archivada |
| Descripción | Explica el propósito de la regla |

Toda Rule puede ser reutilizada por distintos Rule Sets sin perder su identidad.

Las modificaciones nunca alteran una Rule ya publicada; cualquier cambio genera una nueva versión.
---

## Rule Parameter

Un Rule Parameter representa una propiedad configurable perteneciente a una Rule.

Mientras una Rule define qué comportamiento controla el sistema, los Rule Parameters determinan cómo debe comportarse esa regla.

Una Rule puede contener uno o varios Rule Parameters dependiendo de su complejidad.

Ejemplo:

Rule

Máximo de jugadores registrados

Rule Parameters

- Valor actual
- Valor mínimo
- Valor máximo
- Editable
- Visible
- Obligatorio

Otro ejemplo:

Rule

Sistema de puntuación

Rule Parameters

- Victoria = 3
- Empate = 1
- Derrota = 0
- Victoria por default = 3
- Partido suspendido = Configurable

Cada parámetro posee un tipo de dato independiente y puede validarse de manera individual.

Información principal de un Rule Parameter:

| Campo | Descripción |
|--------|-------------|
| Parameter ID | Identificador único |
| Rule ID | Regla a la que pertenece |
| Nombre | Nombre del parámetro |
| Tipo | Integer, Decimal, Boolean, Texto, Fecha, Lista, etc. |
| Valor | Valor almacenado |
| Valor mínimo | Restricción inferior |
| Valor máximo | Restricción superior |
| Obligatorio | Sí / No |
| Editable | Sí / No |
| Visible | Sí / No |
| Orden | Posición dentro de la Rule |
| Descripción | Explicación funcional |

Todos los Rule Parameters pertenecen exclusivamente a una Rule.

 La modificación de un Rule Parameter dentro de una Rule Version deberá realizarse únicamente mediante la creación de una nueva versión del Rule Set.

Las versiones publicadas permanecerán inmutables y conservarán los valores de sus Rule Parameters para garantizar la reconstrucción histórica del reglamento.
------

## Rule Version

Una Rule Version representa una versión específica e inmutable de un Rule Set.

Su propósito es preservar el historial completo de los reglamentos utilizados por las competiciones, garantizando que los cambios futuros nunca alteren el comportamiento de torneos que ya se encuentren en operación.

Cada vez que un organizador modifica un Rule Set publicado, Tournament OS™ crea automáticamente una nueva Rule Version.

Las versiones anteriores permanecen disponibles para consulta histórica y continúan siendo utilizadas por las competiciones que fueron creadas con ellas.

Una Rule Version nunca puede editarse.

Si el organizador necesita realizar cambios adicionales, el sistema generará una nueva versión del reglamento.

Ejemplo de evolución:

Reglamento Liga MX

↓

Versión 1.0

↓

Versión 1.1

↓

Versión 2.0

↓

Versión 2.1

Cada Competition Category queda vinculada mediante un Rule Assignment a una única Rule Version durante su participación en la competencia.

El Rule Assignment determina exactamente qué Rule Version deberá utilizar esa categoría.

Una misma Competition puede utilizar diferentes Rule Versions en sus distintas Competition Categories.

Por ejemplo:

Copa Itzaes 2027

├── Sub-7 → Rule Version Infantil v1.2
├── Sub-9 → Rule Version Infantil v1.2
├── Sub-11 → Rule Version Infantil v1.3
├── Libre → Rule Version Libre v3.0
└── Veteranos → Rule Version Veteranos v2.1

De esta manera, una competencia puede administrar múltiples reglamentos sin duplicar Rules ni Rule Sets.

Información principal de una Rule Version:

| Campo | Descripción |
|--------|-------------|
| Rule Version ID | Identificador único |
| Rule Set ID | Reglamento al que pertenece |
| Número de versión | Ejemplo: 1.0, 1.1, 2.0 |
| Fecha de creación | Momento en que fue generada |
| Creado por | Usuario responsable |
| Estado | Draft, Publicada, Activa, Archivada |
| Basada en | Versión anterior |
| Total de Rule Categories | Cantidad de categorías |
| Total de Rules | Cantidad total de reglas |
| Notas de versión | Resumen de los cambios realizados |

Características principales:

- Es completamente inmutable.
- Conserva el historial completo del reglamento.
- Puede ser utilizada simultáneamente por múltiples competiciones.
- Nunca reemplaza versiones anteriores.
- Permite reconstruir exactamente cualquier torneo histórico.
- Constituye la unidad oficial de auditoría del Competition Rules Engine.

---

## Rule Assignment

Un Rule Assignment representa la asignación oficial de una Rule Version a una Competition Category.

Su propósito es indicar qué reglamento utilizará una categoría específica durante toda la competencia.

El Rule Assignment evita duplicar reglamentos dentro de las competiciones.

En lugar de copiar todas las Rules, la Competition Category mantiene únicamente una referencia hacia la Rule Version asignada.

Una vez iniciada la participación de una Competition Category, su Rule Assignment queda bloqueado.

El bloqueo deberá activarse cuando ocurra cualquiera de las siguientes condiciones:

- La Competition Category haya iniciado oficialmente su participación.
- Exista al menos un partido oficial registrado para la Competition Category.

Después del bloqueo, el Rule Assignment no podrá cambiarse.

Si el organizador requiere utilizar un reglamento diferente para una categoría que todavía no ha iniciado su participación, podrá asignar una Rule Version diferente siempre que las validaciones correspondientes sean aprobadas.

Una categoría que ya tenga participación oficial no podrá cambiar retrospectivamente la Rule Version utilizada.

No es posible cambiar el reglamento de una categoría que ya tenga partidos oficiales registrados.

Si el organizador requiere utilizar un reglamento diferente, deberá crear una nueva Rule Version y asignarla únicamente a categorías que aún no hayan iniciado su participación.

Cada Competition Category puede utilizar un reglamento distinto, permitiendo que una misma competencia administre múltiples categorías con diferentes reglas deportivas.

Información principal de un Rule Assignment:

| Campo | Descripción |
|--------|-------------|
| Rule Assignment ID | Identificador único |
| Competition ID | Competencia propietaria |
| Competition Category ID | Categoría que utilizará el reglamento |
| Rule Version ID | Versión del reglamento asignada |
| Fecha de asignación | Momento de la asignación |
| Asignado por | Usuario responsable |
| Estado | Activo, Finalizado, Cancelado |
| Observaciones | Comentarios administrativos |

**Regla de integridad:** `Competition Category ID` debe pertenecer a `Competition ID` dentro del mismo Assignment. El sistema deberá validar esta relación antes de crear el registro.

Características principales:

- Cada Competition Category posee un único Rule Assignment activo.
- Un mismo Rule Version puede utilizarse simultáneamente por múltiples categorías.
- El Rule Assignment nunca modifica el reglamento.
- El historial completo de asignaciones permanece disponible para auditoría.
- El Rule Assignment garantiza que todas las decisiones deportivas de una categoría se ejecuten utilizando exactamente la misma versión del reglamento.

Ejemplo:

Copa Itzaes 2027

├── Sub-7 → Reglamento Infantil v1.2

├── Sub-9 → Reglamento Infantil v1.2

├── Sub-11 → Reglamento Infantil v1.3

├── Libre → Reglamento Libre v3.0

└── Veteranos → Reglamento Veteranos v2.1

Gracias a este modelo, Tournament OS™ permite que una sola competencia administre múltiples categorías con reglamentos completamente diferentes sin duplicar información.

### Principio Fundamental de Rule Assignment

El Rule Assignment es el vínculo oficial entre una Competition Category y una Rule Version.

El Rule Assignment no contiene Rules ni duplica la configuración del reglamento.

Su única responsabilidad es determinar qué Rule Version debe utilizar una Competition Category.

Una misma Rule Version puede ser asignada simultáneamente a múltiples Competition Categories.

Cada Competition Category solamente podrá tener un Rule Assignment activo.

La Rule Version asignada deberá permanecer inmutable durante toda la participación oficial de la categoría.
---

## Rule Override

Un Rule Override representa una modificación específica de una Rule o de uno de sus Rule Parameters dentro del contexto de una Competition Category.

Su propósito es permitir que una categoría utilice una Rule Version existente como reglamento base y modifique únicamente determinados parámetros sin crear un nuevo Rule Set.

El Rule Override no modifica la Rule original ni la Rule Version asignada.

El Rule Override tampoco crea ni modifica una Rule Version.

El Override representa únicamente una configuración específica aplicada sobre la Rule Version asignada a una Competition Category.

La Rule Version continúa siendo inmutable.

La configuración efectiva se obtiene durante la resolución del reglamento combinando la Rule Version asignada con los Rule Overrides permitidos para la categoría.

Por lo tanto, el flujo conceptual será:

Rule Version

↓

Rules

↓

Rule Parameters

↓

Rule Override

↓

Configuración efectiva de Competition Category

La modificación únicamente aplica dentro de la Competition Category donde fue definida.

Esto permite reutilizar reglamentos completos evitando duplicar Rules y Rule Sets.

---

### Principios de Rule Override

Los Rule Overrides deberán cumplir los siguientes principios:

- Nunca modifican la Rule original.
- Nunca modifican la Rule Version original.
- Únicamente afectan a la Competition Category donde fueron definidos.
- Deben estar asociados a un Rule Assignment.
- Deben conservar historial.
- Deben poder identificarse durante una auditoría.
- Deben respetar las validaciones definidas por la Rule original.
- Solamente pueden modificar parámetros configurables.
- No pueden modificar propiedades estructurales de una Rule.

---

### Herencia y Override

Cuando una Competition Category recibe un Rule Assignment, el sistema obtiene inicialmente todas las Rules y sus valores desde la Rule Version asignada.

Posteriormente, el Competition Rules Engine identifica los Rule Overrides existentes para esa categoría y aplica las modificaciones permitidas.

El orden de resolución será:

Rule Version

↓

Rules

↓

Rule Parameters

↓

Rule Override

↓

Configuración efectiva de la Competition Category

La configuración efectiva representa los valores que realmente utilizará el Competition Rules Engine durante la ejecución de la competencia.

---

### Ejemplo de Herencia

Reglamento base:

Reglamento Infantil v1.2

| Rule | Valor |
|--------|--------|
| Máximo de jugadores | 20 |
| Duración del partido | 25 minutos |
| Sustituciones | Ilimitadas |
| Puntos por victoria | 3 |
| Puntos por empate | 1 |

Categoría:

Sub-11

Rule Overrides:

| Rule | Valor heredado | Valor override |
|--------|--------|--------|
| Máximo de jugadores | 20 | 22 |
| Duración del partido | 25 minutos | 30 minutos |

La configuración efectiva de Sub-11 será:

| Rule | Valor efectivo |
|--------|--------|
| Máximo de jugadores | 22 |
| Duración del partido | 30 minutos |
| Sustituciones | Ilimitadas |
| Puntos por victoria | 3 |
| Puntos por empate | 1 |

Las Rules que no tengan Override conservarán automáticamente el valor definido por la Rule Version.

---

### Información principal de un Rule Override

| Campo | Descripción |
|--------|-------------|
| Override ID | Identificador único |
| Rule Assignment ID | Asignación donde aplica |
| Competition Category ID | Categoría donde aplica |
| Rule Version ID | Versión del reglamento base |
| Rule ID | Regla que será sobrescrita |
| Parameter ID | Parámetro que será modificado |
| Valor heredado | Valor proveniente de la Rule Version antes de aplicar el override |
| Valor override | Nuevo valor definido por esta Competition Category |
| Motivo | Razón del cambio |
| Creado por | Usuario responsable |
| Fecha de creación | Momento de creación |
| Estado | Activo, Reemplazado, Archivado |

---

### Restricciones de Rule Override

Un Rule Override solamente podrá modificar una Rule cuando:

- La Rule sea configurable.
- El Rule Parameter permita modificaciones.
- El nuevo valor cumpla las validaciones de la Rule.
- El usuario tenga permisos suficientes.
- La Competition Category todavía permita modificaciones de reglamento.

Un Rule Override no podrá:

- Modificar el Rule ID.
- Modificar la definición de la Rule.
- Modificar la Rule Category.
- Eliminar una Rule obligatoria.
- Cambiar el tipo de dato de un Rule Parameter.
- Modificar una Rule Version publicada.
- Afectar otras Competition Categories.
- Afectar otras competiciones.

---

### Prioridad de configuración

Cuando exista más de una fuente de configuración, Tournament OS™ resolverá el valor efectivo utilizando la siguiente prioridad:

1. Rule Override específico de la Competition Category.
2. Valor configurado en la Rule Version.
3. Valor por defecto definido por la Rule.

El sistema nunca deberá modificar físicamente los valores originales.

La configuración efectiva será calculada durante la resolución del reglamento.

---

### Auditoría de Rule Override

Cada Rule Override deberá conservar información suficiente para reconstruir quién realizó el cambio, cuándo lo realizó y cuál era el valor anterior.

El historial deberá permitir identificar:

- Usuario que realizó el cambio.
- Fecha y hora.
- Rule afectada.
- Rule Parameter afectado.
- Valor anterior.
- Valor nuevo.
- Motivo del cambio.
- Competition Category donde aplica.
- Rule Version utilizada como base.

Esto permitirá reconstruir exactamente las reglas que estuvieron vigentes durante cualquier competencia histórica.

---

### Ejemplo aplicado a Copa Itzaes

Copa Itzaes 2027

Rule Version:

Reglamento Copa Itzaes v3.0

Categoría:

Sub-11

Rule Overrides:

- Máximo de jugadores: 22
- Duración del partido: 30 minutos

Categoría:

Sub-13

Sin Rule Overrides.

Categoría:

Veteranos

Rule Version:

Reglamento Veteranos v2.1

El resultado es que cada categoría puede utilizar un reglamento base diferente y, cuando sea necesario, modificar únicamente determinados parámetros sin duplicar todo el reglamento.

Este mecanismo permite que Tournament OS™ mantenga una arquitectura de reglamentos reutilizable, flexible y completamente auditable.

---

## Rule Validation

Rule Validation es el componente encargado de verificar que las Rules, Rule Parameters y Rule Versions cumplan con todas las condiciones necesarias antes de ser utilizadas por una competición.

Su objetivo es garantizar la integridad del reglamento y evitar que una configuración inválida pueda afectar el funcionamiento de una competencia.

La validación deberá ejecutarse en diferentes momentos del ciclo de vida del reglamento.

Una Rule Version no podrá ser publicada si contiene errores de configuración o incumple alguna regla obligatoria.

---

### Objetivos de Rule Validation

Rule Validation deberá garantizar que:

- Todas las Rules obligatorias estén configuradas.
- Todos los Rule Parameters obligatorios tengan un valor válido.
- Los valores respeten sus límites mínimos y máximos.
- Los tipos de datos sean correctos.
- Las Rules requeridas existan dentro del Rule Set.
- No existan configuraciones incompatibles.
- Las dependencias entre Rules sean válidas.
- Los Rule Overrides respeten las restricciones permitidas.
- La Rule Version pueda ejecutarse sin errores.
- El reglamento conserve integridad antes de ser publicado.

---

### Momentos de Validación

La validación podrá ejecutarse en diferentes etapas.

#### Validación durante configuración

Se ejecuta mientras el organizador está creando o modificando un Rule Set.

Su objetivo es detectar errores de configuración antes de iniciar el proceso de publicación.

Ejemplos:

- Falta un parámetro obligatorio.
- Un valor se encuentra fuera del rango permitido.
- Una Rule requerida no está configurada.
- Existe una dependencia incompleta.

---

#### Validación antes de publicación

Se ejecuta cuando el organizador intenta publicar una Rule Version.

El sistema deberá realizar una validación completa del reglamento.

Si existen errores críticos, la publicación deberá ser bloqueada.

Una Rule Version solamente podrá pasar a estado Publicado cuando todas las validaciones obligatorias hayan sido aprobadas.

---

#### Validación al realizar una asignación

Antes de crear un Rule Assignment, el sistema deberá verificar que la Rule Version:

- Se encuentre publicada.
- No esté archivada.
- Sea compatible con la Competition Category.
- No contenga errores críticos.
- Se encuentre disponible para ser asignada.

---

#### Validación durante la ejecución

El sistema podrá realizar validaciones adicionales durante la ejecución de las reglas.

Estas validaciones deberán impedir que una operación produzca un resultado incompatible con el reglamento vigente.

---

### Tipos de Validación

Rule Validation podrá utilizar diferentes tipos de validaciones.

#### Validación de tipo

Comprueba que el valor almacenado corresponda al tipo definido por el Rule Parameter.

Ejemplo:

Una Rule de tipo Integer no podrá recibir un valor de texto.

---

#### Validación de rango

Comprueba que el valor se encuentre dentro de los límites establecidos.

Ejemplo:

Mínimo: 1

Máximo: 40

Valor: 28

Resultado:

Válido.

---

#### Validación de obligatoriedad

Comprueba que todos los parámetros marcados como obligatorios tengan un valor.

Ejemplo:

Obligatorio: Sí

Valor: vacío

Resultado:

Inválido.

---

#### Validación de dependencia

Comprueba que una Rule pueda utilizarse únicamente cuando las Rules de las que depende (Rule Dependencies) se encuentren correctamente configuradas.

Ejemplo:

Rule:

Punto adicional por ganar penales

Dependencia:

Permitir serie de penales

La regla no podrá activarse para una competición que no tenga habilitada la Rule "Permitir serie de penales".

---

#### Validación de compatibilidad

Comprueba que diferentes Rules no entren en conflicto.

Ejemplo:

Rule A:

Permitir empate: Sí

Rule B:

Partido requiere ganador: Sí

Resultado:

Configuración potencialmente incompatible.

El sistema deberá solicitar una configuración válida antes de permitir la publicación.

---

#### Validación de Scope

Comprueba que una Rule se utilice únicamente en el nivel para el cual fue definida.

Los principales niveles de Scope podrán incluir:

- Organization
- Competition
- Competition Category
- Team
- Player
- Match
- Phase
- Tournament

Una Rule definida para una Competition Category no deberá aplicarse directamente a una entidad de otro nivel sin una configuración explícita.

---

### Resultado de una Validación

Cada validación deberá generar un resultado.

Los resultados principales serán:

- Válido
- Advertencia
- Error
- Error crítico

#### Válido

La configuración cumple todas las condiciones requeridas.

#### Advertencia

La configuración es válida, pero existe una condición que debería ser revisada por el organizador.

La advertencia no bloquea necesariamente la publicación.

#### Error

Existe una configuración incorrecta que deberá corregirse antes de continuar.

#### Error crítico

La configuración afecta directamente la integridad del reglamento.

Un error crítico deberá impedir la publicación o asignación del Rule Set.

---

### Validation Result

Cada proceso de validación deberá generar un registro de resultado.

Información principal:

| Campo | Descripción |
|--------|-------------|
| Validation ID | Identificador único |
| Rule Set ID | Reglamento validado |
| Rule Version ID | Versión validada |
| Rule ID | Regla relacionada |
| Parameter ID | Parámetro relacionado, si aplica |
| Tipo | Tipo de validación ejecutada |
| Resultado | Válido, Advertencia, Error, Error crítico |
| Mensaje | Descripción del resultado |
| Fecha | Momento de la validación |
| Usuario | Usuario que ejecutó la validación |

Los resultados deberán conservarse para permitir auditoría y diagnóstico.

---

### Reglas para Publicación

Una Rule Version solamente podrá publicarse cuando:

- Todas las Rules obligatorias estén presentes.
- Todos los Rule Parameters obligatorios estén configurados.
- Los valores sean válidos.
- Las dependencias estén satisfechas.
- No existan errores críticos.
- No existan errores de configuración pendientes.
- La estructura del Rule Set sea consistente.
- La versión pueda ser utilizada por el motor de ejecución.

Las advertencias podrán permanecer siempre que la organización permita su publicación bajo dichas condiciones.

---

### Regla de Integridad

El Competition Rules Engine nunca deberá permitir que una configuración inválida se convierta en una Rule Version publicada.

La validación deberá ocurrir antes de que el reglamento pueda afectar una competición.

Una vez publicada y asignada una Rule Version, las validaciones realizadas deberán formar parte del historial de auditoría.

Esto garantiza que Tournament OS™ pueda determinar qué reglas fueron validadas, cuándo fueron validadas y bajo qué configuración fueron utilizadas.

---

### Principio de Validación

La validación debe ser independiente de la interfaz de usuario.

Las mismas reglas de validación deberán ejecutarse aunque la operación sea realizada desde:

- La interfaz web.
- Una aplicación móvil.
- Una API.
- Un proceso automático.
- Una integración externa.

El Competition Rules Engine deberá ser la autoridad final encargada de determinar si una configuración es válida.

---

## Rule Execution

Rule Execution es el componente encargado de ejecutar las reglas definidas en una Rule Version durante el funcionamiento de una competición.

Su función es transformar la configuración de las Rules y sus Rule Parameters en decisiones concretas que puedan ser utilizadas por Tournament OS™.

Rule Execution no modifica las reglas.

Su única responsabilidad es interpretar una Rule Version válida y determinar el resultado correspondiente de acuerdo con el contexto de la operación.

---

### Objetivo de Rule Execution

Rule Execution deberá permitir que Tournament OS™ pueda ejecutar cualquier reglamento sin incorporar reglas deportivas directamente dentro del código de la aplicación.

El motor deberá:

- Identificar la Rule Version aplicable.
- Localizar la Rule correspondiente.
- Obtener sus parámetros.
- Obtener el contexto de la operación.
- Aplicar las validaciones necesarias.
- Evaluar la configuración de la Rule.
- Generar un resultado.
- Registrar la ejecución cuando corresponda.

La ejecución deberá ser determinística.

Ante la misma Rule Version, los mismos parámetros y el mismo contexto, el sistema deberá producir el mismo resultado.

---

### Principio de Ejecución

El Competition Rules Engine separa dos momentos distintos:

#### Momento de publicación (una sola vez por Rule Version)

Rule Version (draft)
↓
Identificar Dependencies declaradas
↓
Validar existencia y compatibilidad de Rules relacionadas
↓
Detectar dependencias circulares
↓
Validar que los Overrides no rompan dependencias obligatorias
↓
Rule Version válida para publicación (resultado cacheado)

#### Momento de ejecución (cada solicitud en runtime)

Rule solicitada
↓
Identificar Rule Assignment
↓
Cargar Rule Version, Rule y Rule Parameters
↓
Aplicar Rule Override si existe
↓
Construir contexto de la operación
↓
Evaluar Rule Conditions contra el contexto
↓
Validar Rule
↓
Ejecutar Rule
↓
Generar resultado
↓
Registrar ejecución

Este es el flujo único y oficial del motor. Las Dependencies **no** se revalidan en cada ejecución — solo al publicar la Rule Version.
---

### Contexto de Ejecución

Una Rule no deberá ejecutarse de manera aislada.

El motor deberá recibir el contexto necesario para determinar si la regla aplica y cuál debe ser su resultado.

El contexto podrá contener información relacionada con:

- Organization
- Competition
- Competition Category
- Team
- Player
- Match
- Phase
- Tournament
- Fecha
- Hora
- Resultado del partido
- Estado de la competencia
- Estadísticas
- Sanciones
- Eventos deportivos

El contexto exacto dependerá del Scope de la Rule.

---

### Ejemplo de Contexto

Rule:

Máximo de jugadores registrados

Rule Version:

Reglamento Infantil v1.2

Rule Parameter:

Valor máximo = 28

Contexto:

Competencia = Copa Itzaes 2027

Categoría = Sub-11

Equipo = Equipo A

Jugadores registrados = 27

Resultado:

Permitido.

Si el equipo intenta registrar un jugador adicional:

Jugadores registrados = 28

Nuevo jugador = Sí

Resultado:

No permitido.

El motor deberá utilizar la misma Rule Version asignada a la categoría para determinar el resultado.

---

### Entrada de Rule Execution

Toda ejecución deberá recibir como mínimo:

| Campo | Descripción |
|--------|-------------|
| Rule Assignment ID | Asignación que determina qué reglamento utilizar |
| Rule Version ID | Versión exacta del reglamento |
| Rule ID | Regla que será ejecutada |
| Contexto | Información necesaria para evaluar la regla |
| Fecha y hora | Momento de ejecución |
| Usuario o proceso | Origen de la solicitud |

La información adicional dependerá del tipo de Rule.

---

### Proceso de Ejecución

Rule Execution seguirá un proceso ordenado.

#### 1. Identificación

El motor identifica la Rule Version correspondiente al Rule Assignment.

No deberá utilizar una versión diferente a la asignada.

---

#### 2. Carga de la Rule

El motor obtiene la Rule que corresponde a la operación solicitada.

La Rule deberá encontrarse disponible y en un estado válido para ejecución.

---

#### 3. Carga de parámetros

El motor obtiene los Rule Parameters asociados a la Rule.

Los valores utilizados deberán corresponder a la Rule Version que está siendo ejecutada.

---

#### 4. Construcción del contexto

El motor reúne la información necesaria para ejecutar la regla.

El contexto deberá contener únicamente la información necesaria para evaluar la decisión.

---

#### 5. Validación

Antes de ejecutar la Rule, el sistema verifica que:

- La Rule exista.
- La Rule Version exista.
- El Rule Assignment sea válido.
- La Rule se encuentre disponible.
- Los parámetros sean válidos.
- El contexto sea suficiente.
- No exista una condición que impida la ejecución.

Si la validación falla, la ejecución deberá detenerse.

---

#### 6. Evaluación

El motor evalúa la Rule utilizando sus parámetros y el contexto recibido.

La evaluación deberá producir un resultado definido por el tipo de Rule.

---

#### 7. Resultado

La ejecución genera un resultado.

Los resultados podrán ser:

- Permitido
- No permitido
- Verdadero
- Falso
- Valor numérico
- Valor de texto
- Lista de valores
- Estado
- Clasificación
- Error de ejecución

El tipo de resultado dependerá de la Rule ejecutada.

---

#### 8. Registro

Cuando corresponda, el sistema deberá registrar la ejecución para fines de auditoría.

El registro deberá permitir conocer:

- Qué Rule fue ejecutada.
- Qué Rule Version fue utilizada.
- Qué contexto fue recibido.
- Qué resultado fue generado.
- Cuándo ocurrió.
- Quién o qué proceso realizó la operación.

---

### Tipos de Ejecución

Las Rules podrán ejecutarse de diferentes formas.

#### Consulta

La aplicación solicita el valor o estado de una Rule.

Ejemplo:

¿Cuál es el máximo de jugadores permitido?

Resultado:

28.

---

#### Validación

La Rule determina si una operación está permitida.

Ejemplo:

¿Puede registrarse este jugador?

Resultado:

No permitido.

---

#### Cálculo

La Rule genera un resultado numérico.

Ejemplo:

Puntos por victoria = 3.

Resultado:

3 puntos.

---

#### Clasificación

La Rule participa en la determinación del orden de equipos.

Ejemplo:

Sistema de desempate:

1. Diferencia de goles.
2. Goles a favor.
3. Resultado entre equipos.

---

#### Restricción

La Rule impide una operación cuando no se cumplen las condiciones establecidas.

Ejemplo:

Un jugador no puede participar si no cumple la edad mínima de la categoría.

---

### Rule Execution Result

Cada ejecución podrá generar un resultado estructurado.

Información principal:

| Campo | Descripción |
|--------|-------------|
| Execution ID | Identificador único |
| Rule Assignment ID | Asignación utilizada |
| Rule Version ID | Versión utilizada |
| Rule ID | Regla ejecutada |
| Contexto | Información utilizada para la ejecución |
| Resultado | Resultado producido |
| Estado | Éxito, Rechazado, Error |
| Fecha y hora | Momento de ejecución |
| Ejecutado por | Usuario, sistema o proceso |
| Duración | Tiempo utilizado para ejecutar la Rule |

El resultado deberá poder ser utilizado por otros componentes de Tournament OS™.

---

### Ejecución Compuesta

Una operación deportiva podrá requerir la ejecución de múltiples Rules.

En estos casos, Tournament OS™ deberá ejecutar las Rules necesarias respetando sus dependencias y prioridades.

Ejemplo:

Registro de jugador

↓

Validar edad

↓

Validar categoría

↓

Validar límite de jugadores

↓

Validar documentación

↓

Validar sanciones

↓

Resultado final

La operación solamente podrá aprobarse cuando todas las Rules obligatorias produzcan un resultado válido.

---

### Prioridad de Ejecución

Cuando una operación requiera múltiples Rules, el sistema deberá respetar el orden definido por las dependencias de las Rules.

Las Rules independientes podrán ejecutarse en paralelo cuando la arquitectura del sistema lo permita.

Las Rules dependientes deberán ejecutarse después de que sus dependencias hayan sido evaluadas correctamente.

---

### Manejo de Errores

Si una Rule no puede ejecutarse correctamente, el motor deberá generar un error controlado.

El error deberá indicar:

- Rule afectada.
- Rule Version utilizada.
- Tipo de error.
- Descripción.
- Contexto relacionado.
- Fecha y hora.

Una ejecución con error no deberá modificar información deportiva de manera parcial.

Cuando una operación requiera múltiples Rules y una Rule crítica falle, la operación completa deberá considerarse fallida.

---

### Inmutabilidad durante la Ejecución

Una Rule Version utilizada durante una ejecución deberá permanecer inmutable.

El motor nunca deberá modificar:

- La Rule Version.
- La Rule.
- Los Rule Parameters.
- El Rule Assignment.

Si el organizador necesita modificar una regla, deberá crear una nueva versión siguiendo el proceso de versionado establecido.

---

### Auditoría de Ejecución

Las ejecuciones que produzcan decisiones deportivas relevantes deberán poder ser auditadas.

El sistema deberá permitir reconstruir:

- Qué reglamento estaba vigente.
- Qué versión fue utilizada.
- Qué Rule fue ejecutada.
- Qué parámetros tenía.
- Qué contexto recibió.
- Qué resultado produjo.
- Cuándo ocurrió la ejecución.
- Quién inició la operación.

Esto permitirá explicar cualquier decisión tomada por Tournament OS™ durante una competencia.

---

### Regla Fundamental de Ejecución

Tournament OS™ nunca deberá determinar una decisión deportiva mediante lógica independiente del Competition Rules Engine cuando dicha decisión esté definida por un reglamento configurable.

Toda decisión regulada deberá pasar por la Rule Version correspondiente.

De esta manera, el código de Tournament OS™ proporciona la capacidad de ejecutar reglas, mientras que el Competition Rules Engine proporciona las reglas que deben ejecutarse.

---

### Ejemplo Completo

Competencia:

Copa Itzaes 2027

Categoría:

Sub-11

Rule Assignment:

Copa Itzaes Infantil v1.3

Rule:

Máximo de jugadores registrados

Rule Parameter:

Valor máximo = 28

Contexto:

Jugadores registrados = 28

Solicitud:

Registrar un nuevo jugador.

Rule Execution:

1. Identificar Rule Assignment.
2. Obtener Rule Version v1.3.
3. Obtener Rule REG-PLAYERS-001.
4. Obtener parámetro Valor máximo = 28.
5. Obtener cantidad actual de jugadores = 28.
6. Evaluar la condición.
7. Generar resultado.

Resultado:

No permitido.

Motivo:

Se alcanzó el máximo de jugadores establecido por el reglamento.

La decisión deberá quedar asociada a la Rule Version utilizada para que pueda ser auditada posteriormente.

---

### Principio Final

El Competition Rules Engine separa completamente la definición de las reglas de su ejecución.

Las Rules definen qué debe cumplirse.

Los Rule Parameters definen los valores de configuración.

Las Rule Versions determinan qué versión del reglamento está vigente.

Los Rule Assignments determinan qué reglamento corresponde a cada categoría.

Rule Validation determina si la configuración es válida.

Rule Execution determina el resultado de aplicar esa configuración a una operación real.

Esta separación permite que Tournament OS™ pueda evolucionar y administrar diferentes reglamentos deportivos sin modificar el código fuente.

---

## Rule Dependencies
Una Rule Dependency representa una relación lógica entre dos o más Rules cuando el funcionamiento de una Rule depende de la configuración o resultado de otra Rule.

Su objetivo es permitir que el Competition Rules Engine pueda expresar relaciones entre reglas sin incorporar dichas relaciones directamente en el código fuente de Tournament OS™.

Una dependencia permite establecer condiciones como:

- Una Rule solamente puede activarse si otra Rule está habilitada.
- Una Rule solamente puede ejecutarse después de otra Rule.
- Una Rule requiere que otra Rule tenga determinado valor.
- Una Rule puede modificar su comportamiento dependiendo del resultado de otra Rule.

---

### Principios de Rule Dependencies

Las dependencias deberán cumplir los siguientes principios:

- Una Rule puede depender de una o varias Rules.
- Una Rule puede ser dependencia de múltiples Rules.
- Las dependencias pertenecen a la configuración del reglamento.
- Las dependencias deberán poder validarse antes de publicar una Rule Version.
- Las dependencias deberán conservarse durante el versionado.
- Las dependencias no deberán modificar las Rules originales.
- Una dependencia inválida deberá impedir la publicación cuando afecte una condición obligatoria.
- El sistema deberá detectar dependencias circulares.

---

### Tipos de Dependencia

El Competition Rules Engine podrá manejar diferentes tipos de relaciones entre Rules.

Las Rule Dependencies representan relaciones estructurales o de ejecución entre Rules.

No deberán utilizarse para representar condiciones basadas directamente en información del contexto de una competencia.

Los principales tipos serán:

#### Depends On

Indica que una Rule depende de la existencia o configuración de otra Rule.

Ejemplo:

Rule:

Punto adicional por ganar penales

Depends On:

Sistema de puntuación por penales

La Rule podrá utilizarse únicamente cuando la Rule requerida forme parte de la configuración correspondiente.

---

#### Requires

Indica que una Rule requiere que otra Rule se encuentre habilitada o configurada para poder utilizarse.

Ejemplo:

Rule:

Punto adicional por ganar penales

Requires:

Definición por penales

Si la Rule de definición por penales no está habilitada, la Rule de punto adicional no podrá utilizarse.

---

#### Conflicts With

Indica que dos Rules no pueden coexistir activamente bajo una misma configuración cuando sus comportamientos sean incompatibles.

Ejemplo:

Rule A:

Empate permitido

Rule B:

Partido requiere ganador

Si ambas Rules generan una configuración incompatible para una misma fase, Rule Validation deberá generar un error.

---

#### Executes After

Indica que una Rule deberá ejecutarse después de otra Rule cuando exista una dependencia de orden de ejecución.

Ejemplo:

Rule:

Asignación de puntos por penales

Executes After:

Resultado de la serie de penales

La Rule de asignación de puntos deberá ejecutarse después de que exista el resultado necesario de la serie.

El orden de ejecución no implica necesariamente que una Rule modifique a otra Rule.

---

### Rule Dependencies y Rule Conditions

Las Rule Dependencies y las Rule Conditions cumplen funciones diferentes.

Una Rule Dependency representa una relación entre Rules.

Una Rule Condition determina si una Rule aplica bajo determinadas circunstancias.

Por lo tanto:

Rule Dependency:

Rule A
   ↓
Requires
   ↓
Rule B

---

### Estructura de una Rule Dependency

Cada dependencia deberá contener la siguiente información.

| Campo | Descripción |
|--------|-------------|
| Dependency ID | Identificador único |
| Rule ID | Rule que contiene la dependencia |
| Rule Version ID | Versión del reglamento donde aplica esta dependencia |
| Related Rule ID | Rule relacionada |
| Tipo | Depends On, Requires, Conflicts With, Executes After
| Condición | Condición que determina la dependencia |
| Valor esperado | Valor requerido para satisfacer la dependencia |
| Obligatoria | Indica si la dependencia es obligatoria |
| Prioridad | Orden de evaluación |
| Estado | Activa, Inactiva, Archivada |
| Descripción | Explicación de la relación |

---

### Ejemplo: Tiempo Extra

Rule:

Permitir tiempo extra

Rule ID:

MATCH-EXTRA-TIME-001

Condition (no Dependency):

Phase.type = Knockout

Contexto:

El valor `Phase.type` proviene de la fase actual del partido, no de otra Rule configurable.

Resultado:

Si `Phase.type = Knockout`, la Rule puede ejecutarse.
Si `Phase.type = GroupStage`, la Rule no aplica.
---

### Ejemplo: Serie de Penales

Rule A:

Permitir serie de penales

Rule ID:

MATCH-PENALTY-SHOOTOUT-001

Rule B:

Punto adicional por ganar penales

Rule ID:

POINTS-PENALTY-WIN-001

Dependency:

Requires

Related Rule:

MATCH-PENALTY-SHOOTOUT-001

La Rule de punto adicional solamente podrá ejecutarse cuando la Rule de definición mediante serie de penales se encuentre habilitada.

La relación entre ambas Rules es una Rule Dependency porque ambas representan comportamientos configurables del reglamento.

La condición:

Serie de penales finalizada = true

no representa una Dependency.

Esta información pertenece al contexto de ejecución y deberá evaluarse mediante una Rule Condition.

Ejemplo:

Torneo A:

Empate = 1 punto

Penales = No

Torneo B:

Empate = 1 punto

Penales = Sí

Ganador de penales = +1 punto

Ambos torneos pueden utilizar el mismo conjunto de Rules sin modificar el código fuente.
---

### Dependencias Condicionales

Una dependencia puede utilizar condiciones basadas en valores de otras Rules.

Ejemplo:

Rule:

Tiempo extra

Valor requerido de la Rule relacionada:

Partido eliminatorio = Sí

Value:

true

La Rule solamente será ejecutable cuando:

Partido eliminatorio = true

Otro ejemplo:

Rule:

Punto adicional por penales

Valor requerido de la Rule relacionada:

Definición por penales = Sí

Value:

true

Si:

Definición por penales = false

La Rule no será ejecutada.

---

### Dependencias entre Parameters

Una dependencia también podrá existir entre Rule Parameters.

Ejemplo:

Parameter:

Máximo de jugadores = 28

Parameter:

Máximo permitido = 40

Validación:

Máximo de jugadores <= Máximo permitido

Si el organizador establece:

Máximo de jugadores = 45

El sistema deberá generar un error de validación.

---

### Orden de Evaluación

Las dependencias deberán evaluarse antes de ejecutar una Rule.

El flujo será:

Rule solicitada

↓

Identificar dependencias

↓

Evaluar Rules relacionadas

↓

Validar condiciones

↓

Determinar si la Rule aplica

↓

Ejecutar Rule

↓

Generar resultado

Una Rule que tenga una dependencia obligatoria no satisfecha no deberá ejecutarse.

---

### Dependencias Circulares

El sistema deberá detectar dependencias circulares.

Ejemplo:

Rule A

↓

depende de Rule B

Rule B

↓

depende de Rule C

Rule C

↓

depende de Rule A

Esta configuración deberá considerarse inválida.

El sistema deberá impedir la publicación de una Rule Version que contenga una dependencia circular obligatoria.

---

### Herencia de Dependencias

Cuando una Rule Version sea versionada a partir de una versión anterior, las Rule Dependencies deberán conservarse como parte de la configuración de la nueva versión.

La nueva Rule Version podrá modificar las dependencias dentro de las reglas permitidas por el sistema.

La versión anterior deberá permanecer completamente intacta.

Ejemplo:

Reglamento Infantil

v1.0

Rule:

Punto adicional por penales

Dependency:

Requires → Serie de penales habilitada

Nueva versión:

Reglamento Infantil

v1.1

La nueva versión puede modificar la configuración de la dependencia si las necesidades del reglamento lo requieren.

La versión v1.0 continuará conservando exactamente la dependencia que tenía cuando fue publicada.

La creación de un reglamento diferente mediante clonación constituye una operación independiente del versionado.

Ejemplo:

Reglamento Infantil v1.0

↓

Clonar

↓

Reglamento Veteranos v1.0

En este caso se crea un nuevo Rule Set y una nueva Rule Version, conservando la referencia histórica al reglamento de origen.

---

### Rule Override y Dependencies

Un Rule Override podrá modificar determinados parámetros de una Rule, pero no deberá romper una dependencia obligatoria.

Ejemplo:

Rule Version:

Reglamento Copa Itzaes v3.0

Rule:

Punto adicional por penales

Dependency:

Serie de penales habilitada

Un Override podrá cambiar:

Punto adicional = 1

a:

Punto adicional = 2

Pero no podrá eliminar la dependencia:

Serie de penales habilitada

Si una modificación intenta romper una dependencia obligatoria, Rule Validation deberá rechazarla.

---

### Validación de Dependencies

Antes de publicar una Rule Version, el sistema deberá verificar:

- Que todas las Rules relacionadas existan.
- Que las Rules relacionadas sean compatibles.
- Que las condiciones sean válidas.
- Que no existan dependencias circulares.
- Que no existan referencias a Rules archivadas cuando sean obligatorias.
- Que los valores esperados sean compatibles con los tipos de datos.
- Que las dependencias obligatorias puedan satisfacerse.
- Que los Rule Overrides no rompan dependencias.

---

### Auditoría

Cada modificación de una dependencia deberá conservar:

- Dependency ID.
- Rule afectada.
- Rule relacionada.
- Tipo de dependencia.
- Condición anterior.
- Condición nueva.
- Usuario que realizó el cambio.
- Fecha y hora.
- Rule Version donde ocurrió el cambio.

Esto permitirá reconstruir cómo estaban relacionadas las Rules durante cualquier competición histórica.

---

### Principio Fundamental

Las dependencias permiten que el Competition Rules Engine describa relaciones complejas entre Rules sin convertir esas relaciones en código específico de una competición.

De esta manera:

Las Rules definen decisiones.

Los Rule Parameters definen valores.

Las Rule Dependencies definen relaciones.

Rule Validation verifica que las relaciones sean válidas.

Rule Execution ejecuta las Rules respetando dichas relaciones.

Esto permite que Tournament OS™ pueda representar reglamentos deportivos complejos manteniendo una arquitectura flexible, reutilizable y escalable.

---

## Rule Conditions
Una Rule Condition representa una condición lógica que determina cuándo una Rule debe aplicarse, activarse o ejecutarse.

Mientras una Rule define qué comportamiento controla el sistema y una Rule Dependency define la relación entre Rules, una Rule Condition define las circunstancias específicas bajo las cuales la Rule será válida.

Las condiciones permiten que un mismo Rule Set pueda adaptarse a diferentes situaciones deportivas sin modificar el código fuente.

---

### Objetivo de las Rule Conditions

Las Rule Conditions permiten expresar decisiones como:

- Si el partido termina empatado, ejecutar una serie de penales.
- Si la fase es de eliminación directa, permitir tiempo extra.
- Si la categoría es Sub-11, utilizar una determinada duración de partido.
- Si un jugador recibe tarjeta roja, generar una suspensión.
- Si una competición utiliza puntos por penales, agregar el punto correspondiente.
- Si el partido pertenece a una fase determinada, aplicar reglas específicas de desempate.

Las condiciones deberán poder evaluarse automáticamente durante la ejecución del reglamento.

---

### Estructura de una Rule Condition

Cada condición deberá contener la siguiente información.

| Campo | Descripción |
|--------|-------------|
| Condition ID | Identificador único |
| Rule ID | Rule a la que pertenece |
| Rule Version ID | Versión del reglamento donde aplica esta condición |
| Parameter ID | Parameter utilizado por la condición, si aplica |
| Operador | Operador lógico utilizado |
| Valor esperado | Valor utilizado para evaluar la condición |
| Valor actual | Valor obtenido del contexto en tiempo de ejecución |
| Resultado | Verdadero / Falso |
| Prioridad | Orden de evaluación |
| Obligatoria | Indica si debe cumplirse |
| Descripción | Explicación de la condición |

---

### Operadores

El Competition Rules Engine deberá permitir diferentes operadores para construir condiciones.

Operadores de comparación:

- Igual a
- Diferente de
- Mayor que
- Mayor o igual que
- Menor que
- Menor o igual que

Operadores lógicos:

- AND
- OR
- NOT

Operadores de existencia:

- Existe
- No existe
- Está habilitado
- Está deshabilitado

---

### Ejemplo básico

Rule:

Permitir tiempo extra

Rule ID:

MATCH-EXTRA-TIME-001

Rule Condition:

Phase.type = Knockout

La condición utiliza información del contexto de la competición.

La Rule solamente podrá ejecutarse cuando la fase actual corresponda a una fase de eliminación directa.

Si: Phase.type = Knockout

Resultado: La Rule puede ejecutarse.

Si: Phase.type = Group

Resultado: La Rule no aplica.

En este caso, Phase.type no representa una Rule Dependency porque corresponde a información del contexto de ejecución.

La relación condicional deberá ser administrada mediante Rule Conditions.
---

### Condiciones basadas en Parameters

Una condición puede utilizar el valor de un Rule Parameter.

Ejemplo:

Rule:

Punto adicional por ganar penales

Parameter:

Definición por penales

Condition:

Definición por penales = Sí

Si el parámetro está habilitado:

Resultado:

Verdadero

La Rule podrá ejecutarse.

Si está deshabilitado:

Resultado:

Falso

La Rule no deberá ejecutarse.

---

### Condiciones Compuestas

Una Rule puede requerir que se cumplan varias condiciones simultáneamente.

Ejemplo:

Rule:

Permitir tiempo extra

Conditions:

Tipo de fase = Eliminación directa

AND

Tiempo extra habilitado = Sí

AND

Partido terminado en empate = Sí

La Rule solamente podrá ejecutarse cuando todas las condiciones sean verdaderas.

---

### Condiciones Alternativas

Una Rule también puede ejecutarse cuando se cumple cualquiera de varias condiciones.

Ejemplo:

Rule:

Partido requiere ganador

Conditions:

Fase = Semifinal

OR

Fase = Final

OR

Fase = Tercer lugar

Si cualquiera de las condiciones es verdadera, la Rule podrá aplicarse.

---

### Condición NOT

El operador NOT permite negar una condición.

Ejemplo:

Rule:

Permitir empate

Condition:

NOT

Fase de eliminación directa

Esto significa:

Si el partido NO pertenece a una fase de eliminación directa, el empate está permitido.

---

### Evaluación de Conditions

Las Rule Conditions deberán evaluarse durante la ejecución de una Rule para determinar si las circunstancias actuales permiten aplicar el comportamiento definido.

El flujo será:

Rule solicitada

↓

Identificar Rule Assignment

↓

Obtener Rule Version

↓

Obtener Rule

↓

Obtener Rule Parameters

↓

Aplicar Rule Overrides

↓

Construir contexto

↓

Identificar Conditions

↓

Obtener valores actuales

↓

Evaluar operadores

↓

Determinar resultado de las Conditions

↓

Validar Rule

↓

Ejecutar Rule

↓

Generar resultado

Una Rule cuya condición obligatoria resulte falsa no deberá ejecutarse.

Las Rule Dependencies estructurales deberán haber sido validadas previamente durante la publicación de la Rule Version.

Durante la ejecución, el motor deberá respetar las condiciones y dependencias que formen parte de la configuración válida de la Rule Version.

---

### Condiciones Dinámicas

Algunas condiciones dependerán de información generada durante una competición.

Ejemplos:

- Resultado final del partido.
- Marcador al finalizar el tiempo reglamentario.
- Número de tarjetas.
- Estado de un jugador.
- Estado de una competición.
- Fase actual.
- Número de goles.
- Resultado de una serie de penales.

Estas condiciones deberán evaluarse utilizando información actualizada del sistema.

---

### Ejemplo Copa Itzaes

Regla:

Punto adicional por ganar serie de penales

Conditions:

Partido terminado en empate = Sí

AND

Definición por penales habilitada = Sí

AND

Serie de penales finalizada = Sí

Resultado:

El equipo ganador de la serie recibe 1 punto adicional.

El equipo que perdió la serie conserva únicamente el punto correspondiente al empate.

De esta manera el sistema puede representar un formato como:

Victoria:

3 puntos

Empate:

1 punto

Victoria en penales:

+1 punto

Derrota:

0 puntos

La configuración puede modificarse mediante Rules y Parameters sin modificar el código fuente.

---

### Condiciones por Scope

Una Condition deberá respetar el Scope de la Rule a la que pertenece.

Los posibles niveles podrán incluir:

- Organización
- Competencia
- Temporada
- Categoría
- Fase
- Grupo
- Partido
- Equipo
- Jugador
- Árbitro

Ejemplo:

Una Rule con Scope = Partido podrá evaluar información específica del partido.

Una Rule con Scope = Jugador podrá evaluar información específica del jugador.

Una Rule con Scope = Categoría podrá evaluar configuraciones generales de la categoría.

---

### Condiciones por Fecha y Hora

Las Conditions podrán utilizar fechas y horarios cuando una regla dependa de un periodo determinado.

Ejemplo:

Rule:

Registro de jugadores habilitado

Condition:

Fecha actual <= Fecha límite de registro

Mientras la condición sea verdadera:

Registro habilitado.

Cuando sea falsa:

Registro cerrado.

---

### Condiciones por Estado

Las Rules también podrán depender del estado de una entidad.

Ejemplo:

Rule:

Permitir modificación de alineación

Condition:

Estado del partido = No iniciado

Si el partido no ha iniciado:

Resultado:

Permitido.

Si el partido ya inició:

Resultado:

No permitido.

---

### Validación de Conditions

Antes de publicar una Rule Version, Rule Validation deberá comprobar:

- Que todas las Conditions tengan una Rule válida.
- Que los Parameters utilizados existan.
- Que los operadores sean compatibles con el tipo de dato.
- Que los valores esperados sean válidos.
- Que no existan condiciones imposibles de cumplir.
- Que las condiciones compuestas tengan una estructura válida.
- Que no existan referencias circulares.
- Que las Conditions sean compatibles con las Rule Dependencies.

---

### Auditoría

Cada modificación de una Condition deberá conservar:

- Condition ID.
- Rule ID.
- Parameter utilizado.
- Operador.
- Valor anterior.
- Valor nuevo.
- Usuario que realizó el cambio.
- Fecha y hora.
- Rule Version donde ocurrió el cambio.

Esto permitirá conocer exactamente qué condiciones estaban configuradas cuando se ejecutó una regla durante una competición histórica.

---

### Principio Fundamental

Las Rule Conditions determinan cuándo una Rule debe aplicarse.

El modelo completo queda de la siguiente manera:

Rules

↓

Definen qué comportamiento controla el sistema.

Rule Parameters

↓

Definen los valores configurables.

Rule Dependencies

↓

Definen las relaciones entre Rules.

Rule Conditions

↓

Definen cuándo una Rule aplica.

Rule Validation

↓

Verifica que la configuración sea válida.

Rule Execution

↓

Ejecuta la Rule cuando corresponde.

Este modelo permite que Tournament OS™ pueda representar diferentes reglamentos, formatos de competición, categorías y sistemas de desempate utilizando la misma infraestructura de reglas.

---

## Rule Templates

Rule Templates representan estructuras predefinidas que permiten crear Rules de manera rápida, consistente y reutilizable.

Su objetivo es evitar que el organizador tenga que definir manualmente desde cero la estructura técnica de Rules que utilizan comportamientos comunes dentro de las competiciones.

Una Rule Template no representa una regla deportiva vigente por sí misma.

Representa una estructura reutilizable a partir de la cual podrá crearse una Rule dentro de un Rule Set.

---

### Objetivos de Rule Templates

Las Rule Templates deberán permitir:

- Crear Rules rápidamente.
- Mantener estructuras consistentes.
- Reducir errores de configuración.
- Reutilizar comportamientos comunes.
- Definir parámetros previamente conocidos.
- Definir validaciones comunes.
- Definir Conditions iniciales.
- Definir Dependencies iniciales.
- Facilitar la creación de nuevos reglamentos.

Las Templates no deberán modificar Rules ya existentes.

---

### Principio de Rule Template

Una Rule Template funciona como una plantilla de creación.

El flujo será:

Rule Template

↓

Crear Rule

↓

Configurar Rule Parameters

↓

Configurar Conditions

↓

Configurar Dependencies

↓

Validar

↓

Agregar a Rule Set

↓

Crear Rule Version

Una vez creada la Rule, ésta tendrá su propia identidad y podrá evolucionar independientemente de la Template utilizada para crearla.

---

### Información principal de una Rule Template

| Campo | Descripción |
|--------|-------------|
| Template ID | Identificador único |
| Nombre | Nombre de la plantilla |
| Categoría | Rule Category recomendada |
| Descripción | Propósito de la plantilla |
| Tipo de Rule | Tipo de comportamiento |
| Parameters | Parámetros iniciales |
| Conditions | Condiciones iniciales |
| Dependencies | Dependencias iniciales |
| Validaciones | Validaciones iniciales |
| Scope | Nivel recomendado de aplicación |
| Estado | Activa, Inactiva, Archivada |
| Versión | Versión de la plantilla |

---

### Ejemplo de Rule Template

Template ID:

TPL-REG-PLAYERS-001

Nombre:

Máximo de jugadores registrados

Categoría:

Registro

Tipo:

Integer

Scope:

Competition Category

Parameters:

- Valor mínimo
- Valor máximo
- Valor actual

Validaciones:

- Valor actual >= Valor mínimo
- Valor actual <= Valor máximo

Valor recomendado:

28

Valor máximo:

40

---

### Creación de una Rule desde una Template

Cuando un organizador selecciona una Rule Template, Tournament OS™ deberá crear una nueva Rule basada en la estructura de la plantilla.

Ejemplo:

Template:

Máximo de jugadores registrados

↓

Nueva Rule:

REG-PLAYERS-001

↓

Rule Parameters:

Valor actual = 28

Valor mínimo = 1

Valor máximo = 40

La Rule creada será independiente de la Template.

Modificar posteriormente la Template no deberá modificar automáticamente las Rules que fueron creadas anteriormente.

---

### Templates de Rules Deportivas

Tournament OS™ podrá proporcionar Templates para comportamientos deportivos frecuentes.

Ejemplos:

#### Registro

- Máximo de jugadores registrados.
- Mínimo de jugadores registrados.
- Máximo de integrantes del cuerpo técnico.
- Edad mínima.
- Edad máxima.
- Número máximo de extranjeros.

#### Competencia

- Duración del partido.
- Número de tiempos.
- Duración del descanso.
- Número de sustituciones.
- Tiempo extra.
- Serie de penales.

#### Puntuación

- Puntos por victoria.
- Puntos por empate.
- Puntos por derrota.
- Punto adicional por penales.
- Puntos por partido ganado por default.

#### Desempates

- Diferencia de goles.
- Goles a favor.
- Resultado entre equipos.
- Fair Play.
- Sorteo.
- Serie de penales.

#### Disciplina

- Tarjeta amarilla.
- Acumulación de tarjetas.
- Tarjeta roja.
- Suspensión automática.
- Suspensión administrativa.

#### Protestas

- Plazo para presentar protesta.
- Requerimiento de documentación.
- Pago de protesta.
- Comité responsable.
- Plazo de resolución.
- Notificación de resolución.

#### Playoffs

- Clasificación directa.
- Repechaje.
- Play In.
- Octavos de final.
- Cuartos de final.
- Semifinal.
- Final.
- Tercer lugar.
- Ida y vuelta.
- Partido único.

---

### Templates Configurables

Una Template podrá establecer valores iniciales, pero el organizador podrá configurarlos cuando cree una Rule.

Ejemplo:

Template:

Puntos por victoria

Valor recomendado:

3

Al crear una Rule:

Valor:

3

Otro reglamento podría utilizar:

Valor:

2

La Template no obliga a utilizar el valor recomendado.

La Rule creada será la responsable del valor definitivo dentro de su Rule Set.

---

### Templates y Rule Categories

Cada Template podrá estar asociada a una Rule Category recomendada.

Esto permitirá que el sistema coloque automáticamente una nueva Rule en el área correspondiente.

Ejemplo:

Template:

Máximo de jugadores registrados

Categoría recomendada:

Registro

Template:

Puntos por victoria

Categoría recomendada:

Puntuación

Template:

Tarjeta roja

Categoría recomendada:

Sanciones

El organizador podrá modificar la categoría cuando la arquitectura del reglamento lo permita.

---

### Templates y Versionado

Las Rule Templates también deberán tener versiones.

Cuando una Template cambie:

Template v1.0

↓

Template v2.0

Las Rules creadas anteriormente no deberán modificarse.

Las nuevas Rules creadas utilizando la Template recibirán la estructura correspondiente a la nueva versión.

Esto permite conservar la trazabilidad histórica.

---

### Templates del Sistema y Templates de Organización

Tournament OS™ podrá manejar diferentes niveles de Templates.

#### System Templates

Templates proporcionadas por Tournament OS™.

Estas podrán representar estructuras deportivas comunes.

Ejemplos:

- Sistema de puntos.
- Desempate.
- Tarjetas.
- Sustituciones.
- Tiempo extra.
- Penales.

#### Organization Templates

Templates creadas por una organización para reutilizar sus propias estructuras de reglamento.

Ejemplo:

Una organización puede crear:

Template:

Reglamento Copa Itzaes - Registro Infantil

y reutilizarla en diferentes temporadas y categorías.

---

### Templates y Reutilización

Una Template podrá utilizarse para crear múltiples Rules.

Ejemplo:

Template:

Máximo de jugadores registrados

↓

Reglamento Infantil

↓

Rule REG-PLAYERS-001

↓

Reglamento Veteranos

↓

Rule REG-PLAYERS-014

Ambas Rules pueden haber sido creadas utilizando la misma Template, pero serán Rules independientes.

---

### Templates y Rule Overrides

Una Rule creada desde una Template podrá posteriormente utilizar Rule Overrides.

Ejemplo:

Template:

Máximo de jugadores registrados

Valor recomendado:

28

Rule Set:

Reglamento Copa Itzaes

Valor:

28

Competition Category:

Sub-11

Rule Override:

22

El valor efectivo para Sub-11 será:

22

La Rule original continuará conservando:

28

---

### Templates y Rule Validation

Una Template podrá incluir validaciones iniciales.

Estas validaciones se copiarán a la Rule cuando ésta sea creada.

Ejemplo:

Template:

Máximo de jugadores

Validación:

Valor mínimo = 1

Valor máximo = 40

Nueva Rule:

Valor actual = 28

La Rule Validation comprobará que:

28 >= 1

y

28 <= 40

Resultado:

Válido.

---

### Templates y Rule Dependencies

Una Template podrá definir Dependencies iniciales.

Ejemplo:

Template:

Punto adicional por ganar penales

Dependency:

Definición por penales habilitada

Al crear la Rule, esta dependencia será incorporada a la nueva Rule.

El organizador podrá modificarla posteriormente dentro de las reglas permitidas por el sistema.

---

### Templates y Rule Conditions

Una Template también podrá proporcionar Conditions iniciales.

Ejemplo:

Template:

Tiempo extra

Condition:

Fase de eliminación directa = Sí

Al crear la Rule:

La Condition será copiada como configuración inicial.

La Rule podrá posteriormente ser configurada de acuerdo con las necesidades del Rule Set.

---

### Inmutabilidad de Templates Utilizadas

Cuando una Rule haya sido creada utilizando una Template, deberá conservarse la referencia histórica de:

- Template ID.
- Template Version.
- Fecha de creación.
- Usuario que creó la Rule.

Esto permitirá conocer el origen de la estructura utilizada para crear una Rule.

La Rule no dependerá operativamente de la Template después de su creación.

---

### Auditoría

El sistema deberá permitir identificar:

- Qué Template originó una Rule.
- Qué versión de la Template fue utilizada.
- Quién creó la Rule.
- Cuándo fue creada.
- Qué parámetros fueron modificados.
- Qué Conditions fueron modificadas.
- Qué Dependencies fueron modificadas.

Esta información permitirá reconstruir el proceso de creación de cualquier reglamento.

---

### Principio Fundamental

Las Rule Templates proporcionan estructuras reutilizables.

Las Rules representan las decisiones configurables.

Los Rule Parameters representan sus valores.

Las Rule Conditions determinan cuándo aplican.

Las Rule Dependencies determinan sus relaciones.

Rule Validation determina si son válidas.

Rule Execution determina el resultado de ejecutarlas.

De esta manera, Tournament OS™ puede proporcionar herramientas para construir reglamentos rápidamente sin sacrificar flexibilidad, versionado, auditoría o independencia entre competiciones.