# Modelo Relacional de Tournament OS™

## 1. Objetivo

El modelo relacional define la estructura de datos persistentes utilizada por Tournament OS™.

Su objetivo es representar las entidades, relaciones, restricciones e historial necesarios para administrar organizaciones, competiciones, categorías, participantes, partidos y reglamentos deportivos.

El modelo deberá permitir que Tournament OS™ mantenga una única fuente de verdad, conserve la integridad histórica y pueda evolucionar sin depender de una competición específica.

Copa Itzaes será la primera implementación real utilizada para validar el modelo, pero ninguna entidad deberá diseñarse exclusivamente para Copa Itzaes.

---

## 2. Principios del Modelo Relacional

### 2.1 Una sola fuente de verdad

Cada dato deberá tener una única entidad propietaria.

Las entidades relacionadas deberán utilizar referencias hacia dicha información en lugar de duplicarla.

---

### 2.2 Integridad referencial

Las relaciones entre entidades deberán impedir referencias inválidas y mantener la consistencia de la información.

---

### 2.3 Independencia entre dominios

Las entidades de organización, competición, participantes, partidos y reglamentos deberán mantener responsabilidades claramente separadas.

---

### 2.4 Inmutabilidad histórica

La información necesaria para reconstruir una competición histórica no deberá depender de modificaciones posteriores realizadas sobre configuraciones actuales.

---

### 2.5 Versionado

Las configuraciones que puedan cambiar durante la evolución de Tournament OS™ deberán contar con mecanismos explícitos de versionado cuando sea necesario conservar su historial.

---

### 2.6 Auditoría

Las operaciones relevantes deberán poder relacionarse con el usuario, proceso, fecha y contexto que las originaron.

---

### 2.7 Escalabilidad

El modelo deberá permitir administrar diferentes organizaciones, competiciones, temporadas, categorías y reglamentos sin modificar su estructura principal.

---

## 3. Entidades Principales

El modelo inicial contempla las siguientes entidades:

### Organización y competición

- Organization
- Competition
- Competition Category
- Season

### Participantes

- Team
- Player
- Coach
- Staff

### Estructura deportiva

- Phase
- Group
- Match
- Match Event
- Standing

### Competition Rules Engine

- Rule Library
- Rule Set
- Rule Category
- Rule
- Rule Parameter
- Rule Version
- Rule Assignment
- Rule Override
- Rule Dependency
- Rule Condition
- Rule Template
- Validation Result
- Rule Execution Result

### Auditoría

- Audit Log

---

# 4. Jerarquía General

La estructura general del modelo será:

Organization
    │
    ├── Rule Library
    │       │
    │       └── Rule Set
    │              │
    │              └── Rule Version
    │
    └── Competition
            │
            ├── Season
            │
            └── Competition Category
                    │
                    ├── Team
                    │
                    ├── Player
                    │
                    ├── Phase
                    │
                    ├── Group
                    │
                    └── Match

---

# 5. Organización

Organization representa a la institución propietaria de la información dentro de Tournament OS™.

Una Organization constituye el límite principal de aislamiento de los datos.

Una organización podrá administrar:

- Competiciones.
- Temporadas.
- Categorías.
- Equipos.
- Jugadores.
- Reglamentos.
- Configuraciones.
- Historial.

Una Organization podrá tener múltiples competiciones.

Ejemplo:

Organization:

Copa Itzaes

Competiciones:

- Copa Itzaes 2027
- Copa Itzaes 2028
- Copa Itzaes 2029

La existencia de una competición no deberá crear una nueva Organization.

---

# 6. Competition

Competition representa una competición deportiva concreta administrada por una Organization.

Una Competition pertenece a una única Organization.

Una Organization puede administrar múltiples Competitions.

Ejemplo:

Organization:

Copa Itzaes

Competition:

Copa Itzaes 2027

Una Competition podrá contener múltiples Competition Categories.

---

# 7. Competition Category

Competition Category representa una categoría deportiva dentro de una Competition.

Una Competition Category pertenece a una única Competition.

Ejemplos:

- Sub-7
- Sub-9
- Sub-11
- Sub-13
- Libre
- Veteranos

Cada Competition Category podrá tener su propia configuración deportiva y su propio Rule Assignment.

Por lo tanto, diferentes categorías de una misma Competition podrán utilizar diferentes Rule Versions.

---

# 8. Season

Season representa el periodo deportivo al que pertenece una competición.

Una Season podrá estar asociada a una o varias competiciones según las necesidades de la organización.

La Season deberá permitir identificar el periodo temporal correspondiente a una competición sin utilizar fechas como sustituto de su identidad.

---

# 9. Team

Team representa un equipo deportivo registrado dentro de Tournament OS™.

Un Team podrá participar en diferentes competiciones o categorías.

La identidad del Team deberá mantenerse independiente de su participación específica en una competición.

La participación de un equipo en una competición deberá representarse mediante una relación específica.

---

# 10. Player

Player representa a una persona que participa como jugador dentro de Tournament OS™.

La identidad del Player deberá mantenerse independiente de una competición específica.

La participación del jugador dentro de un Team o Competition Category deberá representarse mediante relaciones de participación.

Esto permitirá conservar el historial deportivo del jugador sin duplicar su identidad.

---

# 11. Coach y Staff

Coach representa a una persona que desempeña funciones de entrenador.

Staff representa a una persona que desempeña funciones dentro del cuerpo técnico o administrativo de un equipo.

La identidad de estas personas deberá mantenerse separada de su participación específica dentro de una competición.

---

# 12. Phase

Phase representa una fase deportiva dentro de una Competition Category.

Ejemplos:

- Fase de grupos.
- Eliminación directa.
- Repechaje.
- Cuartos de final.
- Semifinal.
- Final.

Una categoría podrá contener múltiples fases.

Las fases deberán mantener un orden y una configuración que permita determinar la estructura deportiva de la competición.

---

# 13. Group

Group representa un grupo deportivo dentro de una Phase.

Ejemplo:

Fase de grupos:

- Grupo A
- Grupo B
- Grupo C
- Grupo D

Un Group podrá contener múltiples equipos participantes y estar relacionado con múltiples partidos.

---

# 14. Match

Match representa un partido oficial dentro de una Competition Category.

Un Match deberá estar relacionado con:

- Competition Category.
- Phase.
- Group cuando corresponda.
- Equipos participantes.
- Fecha y hora.
- Estado.
- Resultado.

La información del partido deberá permitir reconstruir el resultado deportivo de la competición.

---

# 15. Match Event

Match Event representa un evento ocurrido durante un partido.

Ejemplos:

- Gol.
- Tarjeta amarilla.
- Tarjeta roja.
- Sustitución.
- Penal.
- Evento disciplinario.

Los eventos deberán mantener referencia al partido y a las entidades participantes correspondientes cuando aplique.

---

# 16. Standing

Standing representa la posición estadística de los equipos dentro de una clasificación.

La clasificación deberá poder calcularse utilizando los resultados de los partidos y las Rules correspondientes.

La información derivada de una competición no deberá convertirse automáticamente en una segunda fuente de verdad cuando pueda calcularse a partir de los datos originales.

---

# 17. Competition Rules Engine

El Competition Rules Engine representa el sistema encargado de definir, versionar, validar y ejecutar las reglas configurables de Tournament OS™.

Su estructura conceptual se encuentra definida en:

`docs/competition-rules-engine.md`

El modelo relacional deberá implementar las entidades necesarias para conservar esta estructura sin alterar sus principios.

---

# 18. Rule Library

Rule Library representa el repositorio central de reglamentos de una Organization.

Cada Organization posee una única Rule Library.

La Rule Library podrá contener múltiples Rule Sets.

---

# 19. Rule Set

Rule Set representa un reglamento completo.

Un Rule Set pertenece a una Rule Library.

Un Rule Set agrupa las Rule Categories y Rules que forman parte de un reglamento.

Un Rule Set podrá evolucionar mediante Rule Versions sin modificar las versiones históricas.

---

# 20. Rule Category

Rule Category representa una agrupación lógica de Rules relacionadas.

Una Rule Category pertenece a un Rule Set.

Ejemplos:

- Registro.
- Competencia.
- Puntuación.
- Desempates.
- Sanciones.
- Protestas.
- Playoffs.

---

# 21. Rule

Rule representa la unidad mínima de configuración del Competition Rules Engine.

Cada Rule deberá controlar un único comportamiento o decisión de negocio.

Una Rule podrá relacionarse con:

- Rule Parameters.
- Rule Dependencies.
- Rule Conditions.

La identidad de una Rule deberá mantenerse independiente de la configuración específica utilizada por una competición.

---

# 22. Rule Parameter

Rule Parameter representa un valor configurable perteneciente a una Rule.

Una Rule podrá tener uno o varios Parameters.

Los Parameters deberán conservar:

- Tipo de dato.
- Valor.
- Restricciones.
- Estado de configuración.
- Información necesaria para validación.

---

# 23. Rule Version

Rule Version representa una versión específica e inmutable de un Rule Set.

Cada versión deberá conservar la configuración exacta del reglamento correspondiente.

Una Rule Version publicada no deberá modificarse.

Cuando sea necesario realizar cambios, deberá generarse una nueva versión.

---

# 24. Rule Assignment

Rule Assignment representa la asignación de una Rule Version a una Competition Category.

Una Competition Category deberá tener un único Rule Assignment activo.

Una misma Rule Version podrá ser utilizada por múltiples Competition Categories.

El Rule Assignment deberá conservar el historial de la asignación.

---

# 25. Rule Override

Rule Override representa una modificación permitida de un Rule Parameter dentro del contexto de una Competition Category.

Un Override deberá estar asociado a un Rule Assignment.

Un Override nunca deberá modificar físicamente la Rule Version original.

La configuración efectiva deberá resolverse sin alterar los valores originales.

---

# 26. Rule Dependency

Rule Dependency representa una relación lógica entre Rules.

Las dependencias podrán establecer relaciones como:

- Depends On.
- Requires.
- Conditional.
- Conflicts With.
- Executes After.

Las dependencias deberán conservarse dentro del versionado correspondiente y deberán poder validarse antes de la publicación de una Rule Version.

---

# 27. Rule Condition

Rule Condition representa una condición lógica que determina cuándo una Rule aplica o puede ejecutarse.

Las Conditions podrán utilizar:

- Valores de Parameters.
- Información del contexto.
- Estados.
- Fechas.
- Resultados.
- Operadores lógicos.

Las Conditions deberán poder evaluarse durante Rule Execution.

---

# 28. Rule Template

Rule Template representa una estructura reutilizable para crear nuevas Rules.

Una Template no representa una Rule activa.

Su función es proporcionar una estructura inicial que pueda utilizarse para crear Rules.

Las Templates deberán mantener su propio versionado.

Una Rule creada a partir de una Template deberá conservar la referencia histórica de la Template y su versión de origen.

---

# 29. Validation Result

Validation Result representa el resultado generado durante la validación de una Rule, Rule Version, Rule Assignment o configuración relacionada.

Los resultados podrán incluir:

- Válido.
- Advertencia.
- Error.
- Error crítico.

Los resultados deberán conservarse cuando sean necesarios para auditoría y diagnóstico.

---

# 30. Rule Execution Result

Rule Execution Result representa el resultado de ejecutar una Rule dentro de un contexto determinado.

Deberá conservar referencia suficiente para identificar:

- Rule Version.
- Rule.
- Contexto.
- Resultado.
- Fecha y hora.
- Usuario o proceso ejecutor.

El resultado deberá permitir reconstruir decisiones deportivas relevantes.

---

# 31. Audit Log

Audit Log representa el registro histórico de operaciones relevantes realizadas dentro de Tournament OS™.

Deberá permitir identificar, cuando corresponda:

- Entidad afectada.
- Operación realizada.
- Usuario.
- Fecha y hora.
- Valores anteriores.
- Valores nuevos.
- Contexto de la operación.

El Audit Log no deberá sustituir el versionado de las entidades que requieran versiones propias.

---

# 32. Relaciones Fundamentales

Las relaciones principales del modelo serán:

Organization
    │
    ├── 1:1 Rule Library
    │
    └── 1:N Competition

Rule Library
    │
    └── 1:N Rule Set

Rule Set
    │
    ├── 1:N Rule Category
    └── 1:N Rule Version

Rule Category
    │
    └── 1:N Rule

Rule
    │
    ├── 1:N Rule Parameter
    ├── 1:N Rule Dependency
    └── 1:N Rule Condition

Competition
    │
    └── 1:N Competition Category

Competition Category
    │
    ├── 1:N Phase
    ├── 1:N Group
    ├── 1:N Match
    └── 1:1 Rule Assignment

Rule Assignment
    │
    ├── 1:1 Rule Version
    └── 1:N Rule Override

Rule Version
    │
    └── 1:N Rule Assignment

---

# 33. Principio de Integridad

El modelo relacional deberá impedir que una relación permita modificar indirectamente información histórica que deba permanecer inmutable.

Las referencias entre entidades deberán representar relaciones explícitas y no duplicar información que pertenezca a otra entidad.

La estructura física de la base de datos deberá implementar las reglas de integridad definidas por este modelo.

---

# 34. Principio de Evolución

Este documento define el modelo conceptual-relacional inicial.

No constituye todavía el esquema físico de una base de datos.

Las decisiones sobre:

- Tablas físicas.
- Columnas.
- Tipos SQL.
- Claves primarias.
- Claves foráneas.
- Índices.
- Restricciones SQL.
- Migraciones.

se definirán en una etapa posterior.

---

# 35. Estado del Documento

Estado:

Draft

Este modelo deberá revisarse antes de convertirse en esquema físico de base de datos.

Cualquier modificación deberá mantener consistencia con:

- `docs/domain-model.md`
- `docs/business-rules.md`
- `docs/competition-rules-engine.md`
- `ROADMAP.md`