# ADR-0006: Usar Cloudflare D1 y Despliegue Primario en Cloudflare

## Status

Accepted

## Context

El contexto tecnico previo del proyecto mencionaba Supabase/PostgreSQL como
backend real o preferido. El repo de aplicacion aun contiene dependencias,
servicios y migraciones relacionadas con Supabase.

Se decidio cambiar la base de datos objetivo a Cloudflare D1 y orientar el
despliegue web a Cloudflare. El repo de aplicacion ya tiene `wrangler` instalado
como `devDependency`, aunque durante la inspeccion no se encontro un archivo de
configuracion Wrangler visible en la raiz.

## Decision

Usaremos **Cloudflare D1** como base de datos objetivo para Ara World Cup.

Usaremos **Wrangler** como herramienta de plataforma para configurar, migrar y
operar recursos de Cloudflare.

El despliegue primario de la app web sera **Cloudflare**. **Vercel** queda como
fallback si Cloudflare no permite avanzar de forma viable.

Supabase queda como referencia historica y fuente de migracion, no como backend
preferido.

## Considered Options

### Mantener Supabase/PostgreSQL

- Pros: el repo ya contiene cliente, servicios y migracion inicial.
- Cons: contradice la decision vigente y mantiene una dependencia que ya no es
  objetivo arquitectonico.

### Migrar a Cloudflare D1 y desplegar en Cloudflare

- Pros: alinea base de datos y hosting en la plataforma objetivo, aprovecha
  Wrangler y reduce dispersion de infraestructura.
- Cons: exige migrar SQL, reemplazar APIs Supabase y validar compatibilidad de
  Next.js en Cloudflare.

### Desplegar en Vercel y usar D1

- Pros: Vercel puede simplificar deploy de Next.js si Cloudflare se bloquea.
- Cons: deja la plataforma primaria dividida y debe tratarse solo como
  contingencia.

## Consequences

### Positive

- La arquitectura objetivo queda clara para futuras implementaciones.
- La documentacion deja de presentar Supabase como backend preferido.
- Wrangler queda reconocido como herramienta esperada del proyecto.
- Vercel queda definido como fallback, no como decision primaria.

### Negative

- Hay deuda tecnica porque el repo de aplicacion aun contiene integracion
  Supabase.
- D1 puede requerir ajustes frente al schema PostgreSQL previo.
- El despliegue en Cloudflare puede necesitar configuracion o adaptadores
  especificos para Next.js.

## Implementation Notes

- Crear o completar configuracion Wrangler en el repo de aplicacion.
- Crear binding D1 para la app.
- Migrar el schema desde `supabase/migrations/...` hacia migraciones D1.
- Reemplazar o adaptar `lib/supabase/**` y servicios que usan
  `@supabase/supabase-js`.
- Validar Cloudflare como despliegue primario antes de recurrir a Vercel.

## Decision Source

- Decision tecnica indicada por el usuario en el context hub.
- Esta decision se alinea con la regla del proyecto que da mayor peso tecnico a
  Walter Zetino para arquitectura, riesgos, priorizacion y forma de
  implementacion.
