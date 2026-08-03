# Tournament OS™
## Domain Model v1.0

**Versión:** 1.0  
**Estado:** En diseño  
**Fecha:** Agosto 2026

---

# Objetivo

El Domain Model define las entidades principales del negocio y sus relaciones.

Este documento representa la fuente oficial para el diseño de la base de datos, APIs y módulos del sistema.

---

# Entidades principales

## Organización

Representa una empresa o institución que administra uno o varios torneos.

Relaciones:

- Tiene muchos Torneos.
- Tiene muchos Usuarios.
- Tiene muchas Sedes.

---

## Torneo

Representa una competencia organizada.

Ejemplo:

- Copa Itzaes 2027

Relaciones:

- Pertenece a una Organización.
- Tiene Categorías.
- Tiene Jornadas.
- Tiene Árbitros.
- Tiene Equipos.

---

## Categoría

Agrupa equipos por edad, rama o nivel.

Ejemplos:

- Sub-7
- Sub-9
- Sub-11
- Sub-13
- Sub-15
- Sub-17
- Libre
- Femenil

Relaciones:

- Pertenece a un Torneo.
- Tiene muchos Equipos.
- Tiene muchos Partidos.

---

## Equipo

Representa un club participante.

Relaciones:

- Pertenece a una Categoría.
- Tiene muchos Jugadores.
- Tiene Delegados.
- Participa en muchos Partidos.

---

## Jugador

Representa un futbolista registrado.

Relaciones:

- Pertenece a un Equipo.
- Tiene una Credencial.
- Puede participar en muchos Partidos.

---

## Delegado

Responsable administrativo del equipo.

Relaciones:

- Administra un Equipo.

---

## Árbitro

Oficial responsable del partido.

Relaciones:

- Puede dirigir muchos Partidos.

---

## Jornada

Agrupa partidos de una fecha específica.

Relaciones:

- Pertenece a una Categoría.
- Tiene muchos Partidos.

---

## Partido

Representa un encuentro oficial.

Relaciones:

- Equipo Local.
- Equipo Visitante.
- Cancha.
- Árbitro.
- Resultado.
- Estadísticas.

---

## Sede

Lugar físico donde se desarrolla el torneo.

Relaciones:

- Tiene muchas Canchas.

---

## Cancha

Espacio físico donde se juega un partido.

Relaciones:

- Pertenece a una Sede.
- Puede tener muchos Partidos.

---

## Credencial

Documento oficial de identificación.

Relaciones:

- Pertenece a un Jugador.

---

## Usuario

Persona con acceso al sistema.

Tipos:

- Administrador
- Coordinador
- Delegado
- Árbitro
- Capturista

---

# Próxima versión

En la versión 2.0 se definirán:

- Atributos
- Reglas de negocio
- Relaciones detalladas
- Restricciones
- Casos especiales