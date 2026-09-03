---
title: Acompañar la adopción en `hsi`
status: done
created_at: 2026-08-29T00:00:00Z
updated_at: 2026-09-04T03:20:00Z
parent: j
relation.depends: [2j]
---

# Acompañar la adopción en `hsi`

Correr `bilinker init` en el clon de `hsi`, crear el bilink abstracto sobre el fragmento de la API pública, y verificar que `main` no cambia. **Depende de lamansys**, no de nosotros. La funcionalidad ya está terminada y probada en ítem `h`: acá sólo se la usa por primera vez contra un proveedor real.

## Corrida, el 2026-09-02

Se ejecutó **como parte del barrido de [`34`](34.task.md)** y no aparte, porque el barrido incluye este endpoint: `fetchPermissionsFromToken` es uno de los 98, y la biyección contra el censo lo cubre.

Verificado sobre el clon, rama `bilinker-openapi`:

| | |
|---|---|
| `init` corrió | `.bilink/` está en `.git/info/exclude` |
| los bilinks existen | 98, en `refs/bilink/bilinker-openapi` |
| **`main` no cambió** | el árbol de trabajo está limpio y no hay ningún commit de bilinker en la rama |

**Lo que este ítem prometía era el circuito, y el circuito cerró.** Lo que queda del lado de `hsi` —aceptar los 98, y publicar la ref a un remoto que no es nuestro— está anotado en `34`, que es donde vive el barrido.

> El ítem decía *"depende de lamansys, no de nosotros"*. El sprint ya había anotado el 2026-09-02 que eso dejó de ser cierto; esto lo confirma.
