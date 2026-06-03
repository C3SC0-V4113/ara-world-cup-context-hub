# ADR-0007: Usar Identidad Corporativa Microsoft y Acceso Por Roles

## Status

Accepted

## Context

El ecosistema de la empresa se apoya en cuentas corporativas usadas en Windows y
Teams. Los participantes ya estan autenticados en ese entorno, por lo que crear
un sistema de login separado agregaria friccion innecesaria.

La app tambien necesita distinguir usuarios normales de administradores, porque
algunas acciones afectan scoring, resultados, cierres y resoluciones subjetivas.

El repo de aplicacion ya contiene una base tecnica alineada: `auth.ts` usa
`next-auth/providers/microsoft-entra-id`, solicita scopes `openid profile email
User.Read`, y los tipos ya modelan `UserRole = "user" | "admin"` y `avatarUrl`.

## Decision

Usaremos las cuentas corporativas Microsoft/Windows usadas en Teams como proveedor
de identidad para Ara World Cup.

La app debe leer, cuando el proveedor lo permita, los datos basicos del perfil:
identificador estable, nombre, correo y fotografia/avatar.

La app soportara dos roles iniciales:

- `user`: participante normal.
- `admin`: usuario con permisos extra para operar reglas, cierres, resultados,
  scoring y resoluciones subjetivas.

## Considered Options

### Login propio de la app

- Pros: control completo del flujo.
- Cons: duplica identidad corporativa, agrega friccion y aumenta superficie de
  seguridad.

### Login con cuentas corporativas Microsoft

- Pros: aprovecha cuentas existentes, reduce friccion y permite reutilizar datos
  de perfil.
- Cons: depende de configuracion del tenant, scopes y compatibilidad del runtime.

### Roles sin control central

- Pros: implementacion inicial mas rapida.
- Cons: no protege operaciones admin de forma confiable.

## Consequences

### Positive

- Los usuarios ingresan con credenciales que ya usan en el trabajo.
- La app puede precargar nombre, correo y avatar cuando esten disponibles.
- El modelo de permisos queda claro desde el inicio: `user` y `admin`.
- Admin queda conectado con las decisiones de scoring y resolucion manual.

### Negative

- Hay que validar permisos del tenant para fotografia y datos de perfil.
- El despliegue objetivo en Cloudflare puede exigir ajustes al flujo actual de
  NextAuth.
- La asignacion de administradores requiere una fuente controlada.

## Implementation Notes

- Persistir el id estable del proveedor como identificador de usuario.
- Mapear nombre, correo y fotografia a la entidad `User`.
- Usar iniciales o avatar fallback si la fotografia no esta disponible.
- Resolver admins desde tabla/configuracion interna, allowlist o grupo
  corporativo.
- Proteger permisos admin en servidor, no solo en la UI.

## Decision Source

- Decision indicada por el usuario en el context hub.
- La decision se alinea con el ecosistema corporativo basado en Windows y Teams.
- La base tecnica observada en el repo de aplicacion ya apunta a Microsoft Entra
  ID y roles `user`/`admin`.
