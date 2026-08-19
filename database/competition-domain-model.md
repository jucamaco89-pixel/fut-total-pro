# Tournament OS — Competition Domain Model

## 1. Propósito

Este documento define el modelo de dominio de competición de Tournament OS.

Su objetivo es establecer las entidades, relaciones y responsabilidades que permiten representar una competición deportiva desde su configuración inicial hasta su conclusión.

Este modelo deberá mantenerse alineado con:

- `docs/domain-model.md`
- `docs/business-rules.md`
- `docs/competition-rules-engine.md`
- `database/relational-model.md`

El Competition Domain Model define el significado funcional de las entidades de competición.

El modelo relacional define cómo dichas entidades se almacenan en la base de datos.

---

# 2. Principios del dominio

El dominio de competición deberá respetar los siguientes principios:

## 2.1 Identidad

Cada entidad deberá poseer una identidad única dentro del sistema.

La identidad de una entidad no deberá depender exclusivamente de sus atributos descriptivos.

---

## 2.2 Persistencia

Toda entidad persistente deberá poder relacionarse con su representación correspondiente en el modelo relacional.

---

## 2.3 Trazabilidad

Los cambios relevantes realizados sobre una competición deberán poder rastrearse mediante su historial correspondiente.

---

## 2.4 Configuración independiente de ejecución

La configuración de una competición deberá poder definirse antes de iniciar su ejecución.

Una vez iniciada la competición, los cambios deberán respetar las reglas de integridad y las restricciones definidas por el sistema.

---

## 2.5 Reutilización

Las estructuras de competición deberán poder reutilizarse para diferentes tipos de torneos sin modificar la arquitectura principal.

---

# 3. Competition

`Competition` representa una competición deportiva dentro de Tournament OS.

Es la entidad raíz del dominio competitivo.

Una Competition define el contexto general dentro del cual se desarrollará una competición.

### Responsabilidades

- Identificar la competición.
- Mantener su nombre y descripción.
- Definir su estado.
- Relacionarse con su temporada correspondiente.
- Relacionarse con su configuración competitiva.
- Contener las estructuras necesarias para su ejecución.

### Estados

Una Competition podrá encontrarse en estados como:

- DRAFT
- CONFIGURED
- ACTIVE
- COMPLETED
- CANCELLED

El conjunto definitivo de estados deberá estar definido por las reglas de negocio correspondientes.

---

# 4. Season

`Season` representa el periodo deportivo dentro del cual puede desarrollarse una competición.

Una temporada permite organizar competiciones dentro de un marco temporal común.

### Responsabilidades

- Identificar la temporada.
- Definir su periodo.
- Relacionarse con una o más competiciones.
- Permitir distinguir diferentes ciclos deportivos.

Una Competition podrá pertenecer a una Season.

---

# 5. Competition Type

`Competition Type` representa la modalidad estructural de una competición.

Permite determinar cómo deberá organizarse la competición.

Ejemplos:

- Liga.
- Eliminación directa.
- Fase de grupos.
- Grupos más eliminación.
- Formato híbrido.

El Competition Type no deberá contener reglas específicas de una competición concreta.

Las reglas deberán ser proporcionadas mediante el Competition Rules Engine.

---

# 6. Competition Configuration

`Competition Configuration` representa la configuración específica utilizada por una competición.

Puede incluir:

- Número de participantes.
- Número de grupos.
- Número de fases.
- Sistema de puntuación.
- Criterios de clasificación.
- Configuración de desempates.
- Configuración de tiempos extra.
- Configuración de series de penales.
- Restricciones de registro.
- Reglas aplicables.

La configuración deberá quedar vinculada a una versión determinada de las reglas correspondientes.

---

# 7. Competition Participant

`Competition Participant` representa a una entidad que participa en una competición.

Un participante puede representar, dependiendo del contexto:

- Equipo.
- Club.
- Academia.
- Selección.
- Universidad.
- Otro tipo de organización deportiva admitida por el sistema.

La identidad del participante dentro de una competición deberá mantenerse separada de la identidad global de la organización deportiva.

---

# 8. Competition Registration

`Competition Registration` representa la inscripción formal de un participante en una competición.

Permite separar:

- La existencia del participante.
- Su inscripción.
- El estado de dicha inscripción.
- La información específica utilizada por la competición.

### Estados posibles

- PENDING
- APPROVED
- REJECTED
- WITHDRAWN
- DISQUALIFIED

El conjunto definitivo de estados deberá estar determinado por las reglas de negocio.

---

# 9. Competition Phase

`Competition Phase` representa una fase estructural de una competición.

Una competición puede estar formada por una o varias fases.

Ejemplos:

- Fase de grupos.
- Fase regular.
- Octavos de final.
- Cuartos de final.
- Semifinal.
- Final.

Una fase deberá tener una posición definida dentro de la estructura de la competición.

---

# 10. Competition Stage

`Competition Stage` representa una subdivisión operativa dentro de una fase.

Su utilización dependerá del formato competitivo.

Puede representar estructuras como:

- Grupo A.
- Grupo B.
- Conferencia.
- División.
- Llave.
- Subgrupo.

No todas las competiciones necesitarán utilizar Stage.

---

# 11. Group

`Group` representa un conjunto de participantes que compiten dentro de una estructura agrupada.

Un Group pertenece a una Competition Phase o a la estructura equivalente definida por el formato.

Ejemplos:

- Grupo A.
- Grupo B.
- Grupo C.
- Grupo D.

La pertenencia de un participante a un Group deberá registrarse mediante una entidad de relación.

---

# 12. Group Membership

`Group Membership` representa la pertenencia de un participante a un grupo determinado.

Esta entidad permite registrar:

- Participante.
- Grupo.
- Posición inicial cuando aplique.
- Estado de la asignación.
- Información temporal relevante.

La pertenencia a un grupo no deberá modificar la identidad global del participante.

---

# 13. Matchday

`Matchday` representa una jornada o bloque temporal de competición.

Una fase podrá contener múltiples Matchdays cuando el formato competitivo lo requiera.

Puede utilizarse para representar:

- Jornada 1.
- Jornada 2.
- Jornada 3.
- Jornada de semifinales.
- Jornada de final.

No todas las estructuras competitivas necesitarán utilizar Matchday.

---

# 14. Match

`Match` representa un encuentro deportivo programado dentro de una competición.

Es una de las entidades operativas principales del dominio.

Un Match deberá poder relacionarse con:

- Competition.
- Phase.
- Stage.
- Group cuando corresponda.
- Matchday cuando corresponda.
- Participantes.
- Venue cuando corresponda.
- Fecha y hora programadas.
- Estado del partido.
- Resultado.

---

# 15. Match Participant

`Match Participant` representa la participación de un participante dentro de un Match.

Permite determinar:

- Participante local.
- Participante visitante.
- Posición dentro del partido.
- Estado de participación.

La estructura deberá permitir que el sistema no dependa exclusivamente de los conceptos "local" y "visitante" cuando un formato competitivo futuro requiera otra representación.

---

# 16. Match Result

`Match Result` representa el resultado deportivo registrado para un Match.

Puede contener información como:

- Marcador.
- Ganador.
- Estado del resultado.
- Tiempo reglamentario.
- Tiempo extra.
- Definición por penales cuando corresponda.

El resultado deberá distinguir entre el marcador registrado y la forma mediante la cual se determinó el ganador.

---

# 17. Match Event

`Match Event` representa un evento ocurrido durante un partido.

Ejemplos:

- Gol.
- Tarjeta.
- Sustitución.
- Incidencia.
- Otro evento deportivo soportado por el sistema.

Los eventos deberán mantener una relación con el Match correspondiente.

La definición detallada de eventos deportivos deberá permanecer separada del núcleo estructural de competición cuando corresponda.

---

# 18. Standing

`Standing` representa la clasificación de participantes dentro de una estructura competitiva.

Puede corresponder a:

- Competición.
- Fase.
- Grupo.
- Otra estructura clasificatoria.

Una Standing deberá permitir representar los valores utilizados para ordenar participantes.

Ejemplos:

- Partidos jugados.
- Victorias.
- Empates.
- Derrotas.
- Goles a favor.
- Goles en contra.
- Diferencia de goles.
- Puntos.

Los criterios exactos de cálculo deberán ser definidos por el Competition Rules Engine.

---

# 19. Ranking Position

`Ranking Position` representa la posición calculada de un participante dentro de una Standing.

La posición no deberá considerarse una propiedad permanente del participante.

Debe ser un resultado derivado de:

- Resultados.
- Reglas de puntuación.
- Criterios de desempate.
- Estado de la competición.

---

# 20. Qualification

`Qualification` representa la clasificación de un participante desde una estructura competitiva hacia otra.

Puede utilizarse para representar:

- Clasificación a la siguiente fase.
- Clasificación como líder de grupo.
- Clasificación como segundo lugar.
- Clasificación mediante posición.
- Clasificación mediante criterio específico.

La Qualification deberá estar determinada por reglas y no por lógica fija dentro de la interfaz.

---

# 21. Bracket

`Bracket` representa la estructura visual y lógica de una competición de eliminación.

Puede contener:

- Llaves.
- Rondas.
- Enfrentamientos.
- Posiciones de clasificación.
- Conexiones entre partidos.

El Bracket deberá ser una representación de la estructura competitiva y no sustituir las entidades Match o Qualification.

---

# 22. Bracket Round

`Bracket Round` representa una ronda dentro de un Bracket.

Ejemplos:

- Round of 16.
- Quarterfinal.
- Semifinal.
- Final.

Una ronda puede contener uno o varios Match.

---

# 23. Advancement

`Advancement` representa el avance de un participante desde una estructura competitiva hacia otra.

Puede producirse como consecuencia de:

- Resultado de un Match.
- Posición en Standing.
- Regla de clasificación.
- Desempate.
- Resultado de una serie.

El Advancement deberá poder determinarse automáticamente mediante las reglas configuradas.

---

# 24. Venue

`Venue` representa el lugar donde puede desarrollarse un Match.

Puede contener:

- Nombre.
- Dirección.
- Información de disponibilidad.
- Datos operativos.

Un Venue podrá utilizarse en múltiples competiciones y partidos.

---

# 25. Competition Schedule

`Competition Schedule` representa la programación temporal de una competición.

Puede incluir:

- Fecha.
- Hora.
- Jornada.
- Partido.
- Venue.
- Estado de programación.

La programación deberá permanecer separada de la identidad del Match.

Esto permitirá modificar una programación sin modificar la identidad del encuentro.

---

# 26. Competition Rule Set

`Competition Rule Set` representa el conjunto de reglas aplicable a una competición.

Un Rule Set agrupa Rules que determinan el comportamiento competitivo.

La definición de Rule y Rule Set pertenece al Competition Rules Engine.

Este modelo únicamente establece su relación con las entidades de competición.

---

# 27. Rule Version

`Rule Version` representa una versión específica de las reglas utilizadas por una competición.

Una competición deberá poder identificar qué versión de reglas fue utilizada.

Esto permite:

- Reproducibilidad.
- Auditoría.
- Trazabilidad.
- Evolución de reglas.
- Comparación entre versiones.

Una competición activa no deberá cambiar silenciosamente la versión de reglas utilizada para procesar resultados ya registrados.

---

# 28. Competition State

El estado de una competición representa su situación actual dentro de su ciclo de vida.

El estado deberá ser controlado mediante reglas de transición.

Las transiciones deberán impedir operaciones incompatibles con el estado actual.

Ejemplo:

Una competición COMPLETED no deberá aceptar nuevos resultados ordinarios.

---

# 29. Domain Relationships

Las relaciones conceptuales principales son:

```text
Season
  |
  └── Competition
        |
        ├── Competition Configuration
        |
        ├── Competition Registration
        |       |
        |       └── Competition Participant
        |
        ├── Competition Phase
        |       |
        |       ├── Competition Stage
        |       |
        |       └── Group
        |               |
        |               └── Group Membership
        |
        ├── Matchday
        |
        ├── Match
        |     |
        |     ├── Match Participant
        |     ├── Match Result
        |     └── Match Event
        |
        ├── Standing
        |
        ├── Qualification
        |
        ├── Bracket
        |     |
        |     └── Bracket Round
        |
        ├── Advancement
        |
        ├── Competition Schedule
        |
        └── Competition Rule Set
                |
                └── Rule Version