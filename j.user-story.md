---
title: `hsi` publica una abstracción sin cambiar su `main`
status: done
created_at: 2026-08-29T00:00:00Z
updated_at: 2026-09-02T23:45:00Z
parent: 1
---

# `hsi` publica una abstracción sin cambiar su `main`

**Lado:** adopción · **Depende de:** la US 06

Como equipo de `hsi`, quiero declarar que un fragmento de mi API pública es un punto de anclaje, sin que eso cambie un byte de mi rama principal ni me obligue a saber quién lo consume.

**Depende de lamansys, no de nosotros.** Es el primer bloqueo externo de la épica, y por eso está aislado en su propia US en vez de escondido adentro de una task.

## Por qué es así

[ADR-0005](https://github.com/anibalanto/bilinker/blob/ac31d37732d421e14856b9db7db848c1ed74e8da/docs/adr/0005-frontera-entre-proyectos.md) Decisión 1 (el endpoint `abstract`) y [ADR-0004](https://github.com/anibalanto/bilinker/blob/ac31d37732d421e14856b9db7db848c1ed74e8da/docs/adr/0004-bilinks-en-ref-paralela.md) (que es lo que hace que `main` no cambie).

## Cuándo está hecha

No tiene escenarios propios: el comportamiento ya está cubierto por la US 06. Está hecha cuando `hsi` tiene su `refs/bilink/rc-2.32` con un bilink abstracto publicado, y `git diff` sobre su `main` antes y después no muestra nada.

## Cerró el 2026-09-02

Sus tres tasks cerraron —[`1d`](1d.task.md), [`2s`](2s.task.md) y [`k`](k.task.md)— y el sprint ya lo daba por cerrado en prosa; el `status` había quedado atrás.

Publicar una abstracción no cambia un byte de la rama de `hsi`: `.bilink/` va a `.git/info/exclude` y los 98 bilinks viven en `refs/bilink/bilinker-openapi`.
