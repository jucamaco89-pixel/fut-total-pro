# Tournament OS — Database Constraints

## 1. Propósito

Este documento define las restricciones de integridad que deberán proteger la información persistida de Tournament OS.

Su objetivo es garantizar que los datos almacenados en la base de datos mantengan:

- Integridad.
- Consistencia.
- Unicidad.
- Trazabilidad.
- Relaciones válidas.
- Estados válidos.
- Dependencias coherentes.

Este documento deberá mantenerse alineado con:

- `docs/domain-model.md`
- `docs/business-rules.md`
- `docs/competition-rules-engine.md`
- `database/relational-model.md`
- `database/competition-domain-model.md`

Las restricciones aquí definidas representan reglas de integridad de datos.

No sustituyen las reglas de negocio ni el Competition Rules Engine.

---

# 2. Principios de integridad

## 2.1 Integridad referencial

Toda relación entre entidades deberá mantener una referencia válida.

Una entidad no deberá apuntar a un registro inexistente.

Las relaciones deberán implementarse mediante claves foráneas cuando correspondan.

---

## 2.2 Identidad única

Cada entidad persistente deberá poseer una identidad única.

La identidad técnica de un registro no deberá depender exclusivamente de atributos descriptivos.

---

## 2.3 Unicidad

Los atributos que representen identificadores naturales dentro de un contexto determinado deberán mantener restricciones de unicidad.

La unicidad deberá definirse considerando el ámbito correspondiente.

Ejemplo:

Un nombre de competición no necesariamente tiene que ser globalmente único.

Una identificación oficial de competición dentro de una organización sí puede requerir unicidad dentro de dicha organización.

---

## 2.4 Integridad de obligatoriedad

Los atributos indispensables para mantener la identidad o integridad de una entidad deberán ser obligatorios.

No deberán permitirse valores NULL cuando la ausencia del valor haga imposible interpretar correctamente el registro.

---

## 2.5 Integridad de estados

Los estados persistidos deberán pertenecer únicamente al conjunto de estados válidos definido por el dominio correspondiente.

No deberán almacenarse estados arbitrarios.

---

# 3. Primary Keys

Toda entidad persistente deberá tener una clave primaria.

Las claves primarias deberán:

- Identificar un único registro.
- Ser estables.
- No depender de información que pueda cambiar.
- Permitir referencias desde otras entidades.

La implementación concreta del tipo de identificador deberá definirse en la implementación de persistencia.

---

# 4. Foreign Keys

Toda relación persistente entre entidades deberá utilizar una clave foránea cuando corresponda.

Las claves foráneas deberán garantizar que:

```text
registro relacionado
        ↓
registro padre existente

5. Competition Integrity

Una Competition deberá mantener una identidad válida y consistente.

Toda Competition deberá:

Tener identificador.
Tener estado válido.
Tener la configuración requerida por su estado.
Mantener una relación válida con su Season cuando dicha relación sea obligatoria.
Mantener una configuración de reglas válida cuando la competición se encuentre lista para ejecutarse.

Una Competition no deberá pasar a estado operativo si no cumple las condiciones mínimas de configuración.

6. Season Integrity

Una Season deberá mantener:

Identificador único.
Nombre o identificador funcional.
Periodo temporal válido.

Cuando se definan fechas de inicio y finalización:

fecha_inicio <= fecha_fin

No deberá existir una Season con un periodo temporal inválido.

7. Competition Configuration Integrity

Una configuración de competición deberá pertenecer a una Competition válida.

No deberá existir una configuración huérfana.

La configuración deberá mantener coherencia con el Competition Type correspondiente.

Los valores configurables deberán respetar las restricciones definidas por el dominio y por el Competition Rules Engine.

La base de datos deberá proteger la integridad estructural de los valores.

Las decisiones de comportamiento deberán permanecer en las reglas de negocio.

8. Competition Registration Integrity

Una inscripción deberá estar asociada a:

Una Competition válida.
Un Participant válido.

No deberá existir una inscripción sin competición.

No deberá existir una inscripción sin participante.

8.1 Unicidad de inscripción

Un mismo Participant no deberá poder tener múltiples inscripciones activas equivalentes dentro de la misma Competition.

La restricción deberá impedir duplicaciones accidentales.

La posibilidad de registrar nuevamente al mismo participante después de una cancelación o retiro deberá resolverse mediante el modelo de estados y las reglas de negocio correspondientes.

9. Participant Integrity

Un Participant deberá mantener una identidad única dentro del contexto correspondiente.

La identidad global del participante deberá permanecer separada de su inscripción específica en una Competition.

Esto permite que un mismo participante pueda participar en múltiples competiciones.

Participant
     |
     ├── Competition A
     ├── Competition B
     └── Competition C

No deberá duplicarse la identidad global del participante para cada competición.

10. Competition Phase Integrity

Una Competition Phase deberá pertenecer a una Competition válida.

Toda Phase deberá mantener una posición o identificación estructural que permita determinar su orden cuando el formato competitivo lo requiera.

No deberán existir fases huérfanas.

10.1 Phase Ordering

Cuando una competición utilice un orden secuencial de fases:

Phase 1
Phase 2
Phase 3
...

la posición deberá ser consistente dentro de la Competition correspondiente.

No deberán existir dos fases con la misma posición cuando dicha posición deba ser única dentro de la competición.

11. Competition Stage Integrity

Una Competition Stage deberá pertenecer a una estructura competitiva válida.

Cuando una Stage dependa de una Phase, deberá existir una referencia válida hacia dicha Phase.

No deberán existir Stage huérfanos.

12. Group Integrity

Un Group deberá pertenecer a una estructura competitiva válida.

Cuando los grupos estén definidos dentro de una Phase:

Competition
    ↓
Phase
    ↓
Group

cada relación deberá mantenerse mediante referencias válidas.

12.1 Group Identity

El identificador funcional de un Group deberá ser único dentro del contexto correspondiente.

Por ejemplo:

Grupo A
Grupo B
Grupo C

No deberá existir una duplicación accidental de la misma identificación dentro de una misma estructura.

13. Group Membership Integrity

Una relación Group Membership deberá referenciar:

Un Group existente.
Un Participant existente o una Competition Registration válida, según el modelo definitivo.

La relación deberá impedir registros duplicados equivalentes.

Un participante no deberá aparecer dos veces dentro del mismo Group mediante registros activos equivalentes.

14. Match Integrity

Todo Match deberá pertenecer a una Competition válida.

Cuando corresponda, también deberá mantener relaciones válidas con:

Phase.
Stage.
Group.
Matchday.
Schedule.
Venue.

No deberán existir partidos huérfanos.

15. Match Participant Integrity

Un Match Participant deberá referenciar un Match existente y un Participant válido.

La relación deberá impedir duplicaciones equivalentes.

Cuando el modelo utilice posiciones como:

HOME
AWAY

estas posiciones deberán estar restringidas a los valores permitidos.

La estructura deberá permitir futuras modalidades que no dependan exclusivamente de HOME/AWAY.

16. Match Result Integrity

Un Match Result deberá pertenecer a un Match válido.

No deberá existir un resultado asociado a un partido inexistente.

El resultado deberá mantener coherencia estructural con los participantes registrados en el Match.

La base de datos deberá proteger las relaciones.

Las reglas para determinar el ganador deberán permanecer en el Competition Rules Engine.

17. Match Event Integrity

Todo Match Event deberá pertenecer a un Match válido.

Un evento no deberá existir fuera del contexto de un partido.

Cuando un evento esté relacionado con un Participant o Player, dicha relación también deberá ser válida.

La base de datos deberá proteger la integridad referencial.

La interpretación deportiva del evento deberá permanecer en el dominio correspondiente.

18. Standing Integrity

Una Standing deberá pertenecer a una estructura competitiva válida.

La Standing deberá poder determinar claramente:

Competition.
Phase cuando corresponda.
Group cuando corresponda.
Participant.

No deberá existir una clasificación huérfana.

18.1 Standing Uniqueness

No deberán existir múltiples registros activos que representen exactamente la misma combinación de:

Competition
Phase
Group
Participant

cuando dicha combinación represente una única posición clasificatoria.

19. Ranking Integrity

La posición de un participante dentro de una Standing deberá pertenecer al contexto correspondiente.

No deberán existir posiciones duplicadas dentro de una clasificación cuando el formato requiera una posición única.

Sin embargo, la posibilidad de posiciones empatadas deberá quedar determinada por las reglas de competición.

La base de datos no deberá imponer una interpretación deportiva que corresponda al Competition Rules Engine.

20. Qualification Integrity

Una Qualification deberá mantener referencias válidas hacia:

Estructura de origen.
Estructura de destino.
Participant cuando corresponda.

No deberá existir una Qualification que apunte hacia una estructura inexistente.

La decisión de quién clasifica deberá ser determinada por las reglas correspondientes.

21. Advancement Integrity

Un Advancement deberá representar una transición válida entre estructuras competitivas.

Deberá poder rastrearse:

estructura origen
        ↓
criterio
        ↓
participante
        ↓
estructura destino

No deberán existir avances hacia estructuras inexistentes.

22. Bracket Integrity

Un Bracket deberá pertenecer a una competición válida.

Cada Bracket Round deberá pertenecer a un Bracket válido.

Los elementos del Bracket deberán mantener relaciones consistentes con los Match correspondientes.

El Bracket no deberá duplicar información que ya tenga como fuente de verdad al Match.

23. Schedule Integrity

Una programación deberá estar asociada a un Match válido cuando represente la programación de un encuentro.

Cuando exista Venue:

Schedule
    ↓
Venue

la referencia deberá ser válida.

Una modificación de horario no deberá crear un nuevo Match cuando el encuentro siga representando la misma identidad competitiva.

24. Venue Integrity

Un Venue deberá mantener una identidad única.

Los datos descriptivos podrán modificarse sin modificar su identidad técnica.

Una eliminación física de un Venue no deberá provocar pérdida de integridad histórica de partidos que ya hayan utilizado dicho Venue.

La estrategia concreta de eliminación:

Restricción.
Soft delete.
Archivado.

deberá definirse en la implementación de persistencia.

25. Rule Set Integrity

Una Competition Rule Set deberá pertenecer a una competición válida cuando represente la configuración específica de dicha competición.

Las Rules asociadas deberán ser identificables.

Una Rule Version utilizada por una competición deberá mantenerse identificable para permitir reproducibilidad y auditoría.

26. Rule Version Integrity

Una Rule Version deberá mantener:

Identidad.
Número o identificador de versión.
Estado correspondiente.
Referencia al Rule o Rule Set cuando corresponda.

Una versión utilizada para procesar información histórica no deberá modificarse de forma destructiva.

Las modificaciones deberán producir una nueva versión cuando las reglas así lo requieran.

27. Historical Integrity

Los datos históricos relevantes deberán permanecer reproducibles.

Cuando una competición haya comenzado a utilizar una determinada configuración o Rule Version, los registros históricos asociados no deberán quedar afectados silenciosamente por cambios posteriores.

La estrategia de versionado deberá proteger:

Resultados.
Clasificaciones.
Reglas utilizadas.
Configuración utilizada.
Decisiones derivadas.
28. Cascade Rules

Las operaciones de eliminación en cascada deberán utilizarse con extrema precaución.

No deberá utilizarse CASCADE DELETE cuando pueda provocar la pérdida de información histórica relevante.

Antes de establecer una eliminación en cascada deberá determinarse:

Si la entidad dependiente tiene valor histórico.
Si la eliminación debe estar permitida.
Si corresponde archivado.
Si corresponde soft delete.
Si la relación debe impedir la eliminación.

La estrategia definitiva deberá establecerse en la implementación de persistencia.

29. Nullability

Los campos deberán clasificarse como:

Obligatorios.
Opcionales.
Condicionales.

Un campo deberá permitir NULL únicamente cuando la ausencia del dato sea válida dentro del dominio.

No deberá utilizarse NULL como sustituto de estados o valores desconocidos cuando el dominio requiera una representación explícita.

30. Temporal Integrity

Las entidades con información temporal deberán mantener coherencia entre sus fechas.

Cuando corresponda:

inicio <= fin

Los registros históricos no deberán permitir modificaciones temporales que produzcan inconsistencias.

Las reglas específicas sobre fechas de competición deberán permanecer en el dominio de negocio.

31. Status Integrity

Los estados almacenados deberán pertenecer al conjunto permitido por la entidad.

Ejemplo:

Competition
    DRAFT
    CONFIGURED
    ACTIVE
    COMPLETED
    CANCELLED

Estos valores son conceptuales.

La enumeración definitiva deberá mantenerse sincronizada con el modelo de dominio y las reglas de negocio.

No deberán almacenarse valores arbitrarios.

32. Soft Delete

Cuando una entidad tenga importancia histórica, deberá evaluarse el uso de eliminación lógica.

La eliminación lógica deberá permitir conservar:

Identidad.
Relaciones históricas.
Auditoría.
Resultados.
Información necesaria para reproducibilidad.

No deberá utilizarse soft delete indiscriminadamente.

La estrategia deberá definirse entidad por entidad.

33. Audit Integrity

Las operaciones críticas deberán poder relacionarse con información de auditoría cuando el sistema lo requiera.

La auditoría deberá permitir identificar, según corresponda:

Qué ocurrió.
Cuándo ocurrió.
Sobre qué entidad ocurrió.
Qué usuario o proceso originó el cambio.

La implementación detallada de auditoría deberá definirse posteriormente.

34. Database vs Business Rules

La base de datos deberá proteger principalmente:

Identidad.
Relaciones.
Unicidad.
Obligatoriedad.
Integridad referencial.
Valores estructuralmente válidos.

La base de datos no deberá convertirse en el único lugar donde se implementen reglas complejas de competición.

Las reglas deportivas deberán permanecer en:

Business Rules
        ↓
Competition Rules Engine
35. Database vs Domain Model

El modelo de dominio define:

qué entidades existen
qué representan
cómo se relacionan

Las restricciones de base de datos definen:

qué combinaciones de datos son estructuralmente válidas

Ambos modelos deberán permanecer sincronizados.

36. Database vs Competition Rules Engine

El Competition Rules Engine podrá determinar decisiones como:

Quién clasifica.
Cómo se asignan puntos.
Cómo se resuelven desempates.
Quién avanza.
Cómo se determina un ganador.

La base de datos deberá garantizar que los datos utilizados para tomar esas decisiones sean estructuralmente válidos.

37. Copa Itzaes

Copa Itzaes será la primera competición real utilizada para validar estas restricciones.

Las necesidades específicas de Copa Itzaes no deberán generar restricciones exclusivas que impidan reutilizar el modelo para otras competiciones.

Cualquier necesidad específica deberá clasificarse como:

Configuración.
Regla.
Relación.
Entidad.
Restricción estructural.

antes de modificar la arquitectura.

38. Regla de consistencia arquitectónica

Cuando exista una contradicción entre:

Modelo de dominio.
Modelo relacional.
Reglas de negocio.
Competition Rules Engine.
Restricciones de base de datos.

la implementación deberá detenerse hasta resolver la contradicción.

No deberán introducirse excepciones silenciosas.

39. Evolución

Las restricciones deberán evolucionar junto con el modelo de dominio.

Una modificación deberá evaluarse en este orden:

Impacto en el modelo de dominio.
Impacto en reglas de negocio.
Impacto en Competition Rules Engine.
Impacto en modelo relacional.
Impacto en restricciones.
Impacto en datos existentes.
Impacto en migraciones.
40. Regla de fuente de verdad

Ninguna restricción deberá duplicar innecesariamente una regla compleja que pertenezca al Competition Rules Engine.

La base de datos deberá proteger la integridad estructural.

El dominio deberá representar el significado.

El Rules Engine deberá determinar el comportamiento competitivo.

41. Estado del documento

Estado:

DRAFT — Architecture Definition

Este documento define las restricciones conceptuales de integridad de datos de Tournament OS.

Antes de implementar las restricciones físicas en una base de datos concreta deberán revisarse conjuntamente:

database/relational-model.md
database/competition-domain-model.md
database/database-constraints.md
docs/domain-model.md
docs/business-rules.md
docs/competition-rules-engine.md

La implementación física de constraints, índices, claves foráneas, enums y migraciones se realizará posteriormente