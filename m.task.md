---
title: Vincular `USER_PERMISSIONS` de `retinar` con `hsi`
status: done
created_at: 2026-08-29T00:00:00Z
updated_at: 2026-09-02T11:50:00Z
parent: l
---

# Vincular `USER_PERMISSIONS` de `retinar` con `hsi`

El caso que motiva la épica. Declarar `.bilink/.hsi.toml`, crear el bilink con endpoint repo, aceptar, y comprobar que el drift de la ruta se reporta. **Depende de ítem `j`.**

## Corrido de punta a punta, el 2026-09-02

### El circuito

```
bilinker init                              en retinar
.bilink/.hsi.toml                          remote + branch = bilinker-openapi
bilinker track main                        la ref del consumidor
bilinker fetch hsi                         el proveedor, sin sacar archivos al árbol
bilinker abstracts hsi                     98 abstracción(es), con su código
bilinker chain new --from-repo hsi:<uuid> --as.0 interface --tip <el método>
bilinker accept --no-n1 .
```

**`remote` apunta al clon local** de `hsi`, porque `refs/bilink/bilinker-openapi` todavía no está publicada — ver [`34`](34.task.md). Es lo único que no se ejercitó: `abstracts` no hace red por diseño, así que el resto del mecanismo corrió igual.

### El vecindario cruzó la frontera

```yaml
  0:
    link: repo hsi
    accepted:
      link: capture d33b50bd…
      hash: dca6e439…
      hash_ast: 1e389614…
      n:
        1:
          hash: 5bc80081…      ← el vecindario de hsi, opaco
```

De esos hashes `retinar` no reconstruye path, query, texto ni commit — que es la propiedad que [`frontier.md`](../../subsystems/bilinker/concepts/frontier.md) promete. **Es la mitad que esta épica existe para entregar**, y es la primera vez que viaja.

### Y el drift se reportó

Se movió la ruta en `hsi` —`/user/person/` → `/user/persona/`—, el proveedor aceptó, y del lado del consumidor:

```
$ bilinker check .
760cc053  (CHAIN_DIRTY, OK)
```

**`CHAIN_DIRTY` en el extremo remoto y `OK` en el local**, que es exactamente la distinción que [`l`](l.user-story.md) pedía: *movió* de un lado, intacto del otro. Y `get` sobre el endpoint remoto muestra la ruta nueva leída del código de `hsi`, sin clonarlo a mano.

Todo revertido después: `hsi` volvió a `98 × (OK, OPEN)` y `retinar` a `all clean`.

### El mismatch quedó a la vista, y es un bug de producción

`retinar` llama a su constante `USER_PERMISSIONS` y la ruta que manda es `/user/person/from-token`:

| | manda | deserializa | `hsi` devuelve |
|---|---|---|---|
| `retinar.fetchUserPermissionsFromToken` | `/user/person/from-token` | `HSIRoleInfoDto[]` | **`FetchUserPersonFromTokenDto`** — un objeto, una persona |
| el endpoint de roles | `/user/permissions/from-token` | | `List<PublicAuthorityDto>` |

**El bilink se creó contra `/person/`, que es lo que el código hace**, no contra lo que el nombre de la constante sugiere: un bilink declara una referencia que existe, no una que convendría.

Lo que lo volvió visible en dos líneas fue `chain list "/public-api/user/"` con los alias de [`3d`](3d.task.md) — antes eran 98 hexadecimales.

**Arreglarlo es de `retinar` y no de acá.** Lo que este ítem tenía que probar es que la herramienta lo muestra, y lo muestra.
