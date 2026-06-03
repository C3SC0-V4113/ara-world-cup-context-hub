# Identidad y Acceso

## Decision vigente

Ara World Cup usara las cuentas corporativas del ecosistema Microsoft como
proveedor de identidad.

La experiencia esperada es que las personas ingresen con la misma cuenta que ya
usan en Windows y Teams. Si el proveedor lo permite, la app debe leer datos
basicos del perfil corporativo:

- identificador estable del proveedor;
- nombre;
- correo;
- fotografia o avatar.

La fotografia es opcional. Si no esta disponible, la app debe usar iniciales u
otro avatar fallback.

## Proveedor objetivo

El proveedor objetivo es Microsoft Entra ID o el mecanismo corporativo equivalente
que respalde las cuentas usadas en Windows y Teams.

Teams es parte del contexto corporativo de identidad y comunicacion, pero no
necesariamente el unico canal de autenticacion visual. La app web puede mostrar
su propio login mientras delega la identidad al proveedor Microsoft.

## Roles iniciales

### `user`

Participante normal del juego.

Permisos esperados:

- ver home, calendario y ranking;
- gestionar sus predicciones generales y diarias;
- gestionar sus selecciones favoritas;
- ver su informacion de perfil disponible.

### `admin`

Usuario con permisos extra para operar el juego.

Permisos esperados:

- administrar reglas de puntuacion;
- operar cierres de predicciones;
- cargar o corregir resultados;
- resolver categorias subjetivas;
- disparar o validar recalculos de scoring y ranking.

## Estado observado del repo de aplicacion

Durante la inspeccion del repo `E:\Repositorios\ara-world-cup` se observo:

- `auth.ts` usa `next-auth/providers/microsoft-entra-id`;
- el flujo solicita scopes `openid profile email User.Read`;
- la sesion mapea `id`, `name`, `email` e `image` cuando el proveedor los entrega;
- `types/index.ts` ya modela `UserRole = "user" | "admin"`;
- `User` ya contempla `avatarUrl` y `role`.

## Implicaciones para implementacion futura

Una implementacion posterior debera:

- mantener o adaptar NextAuth con Microsoft Entra ID, salvo que el runtime de
  Cloudflare obligue a otro enfoque compatible;
- persistir el identificador estable del proveedor como id de usuario;
- mapear nombre, correo y fotografia a la entidad de usuario;
- resolver administradores desde una fuente controlada, como tabla interna,
  allowlist o grupo corporativo;
- proteger acciones admin en servidor, server actions o servicios, no solo
  ocultando botones en UI;
- mantener fallback visual cuando no exista fotografia de perfil.

## Riesgos

- La fotografia puede no estar disponible por permisos, scopes o politicas del
  tenant.
- El runtime de Cloudflare puede requerir ajustes al enfoque actual de NextAuth.
- Si los roles se resuelven solo en cliente, usuarios sin permiso podrian intentar
  invocar acciones administrativas directamente.
- Si el rol admin depende de configuracion manual, debe existir un proceso claro
  para altas y bajas.
