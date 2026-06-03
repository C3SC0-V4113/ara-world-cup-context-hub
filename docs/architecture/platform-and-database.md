# Plataforma y Base de Datos

## Decision vigente

Ara World Cup debe orientarse a:

- **Base de datos objetivo:** Cloudflare D1.
- **Herramienta de plataforma:** Wrangler.
- **Despliegue primario:** Cloudflare.
- **Fallback de despliegue:** Vercel, solo si Cloudflare bloquea el avance.

Esta decision reemplaza el supuesto anterior de Supabase/PostgreSQL como backend
preferido.

## Estado anterior

El repo de aplicacion todavia contiene referencias a Supabase:

- dependencia `@supabase/supabase-js`;
- carpeta `lib/supabase/**`;
- servicios que llaman `getSupabaseServerClient`;
- migracion historica en `supabase/migrations/202605280001_initial_schema.sql`;
- documentacion previa que menciona Supabase como backend real.

Esas referencias deben tratarse como legado o contexto de migracion, no como
arquitectura objetivo.

## Estado observado del repo de aplicacion

Durante la inspeccion del repo `E:\Repositorios\ara-world-cup` se observo:

- `wrangler` ya esta instalado como `devDependency` en `package.json`.
- No se encontro archivo visible de configuracion Wrangler en la raiz del repo.
- La implementacion de servicios aun depende de cliente Supabase o fallback a
  mock data.

## Implicaciones para implementacion futura

Una migracion tecnica posterior debera:

- crear o completar `wrangler.toml` o configuracion equivalente;
- crear el binding de Cloudflare D1 para la app;
- convertir la migracion Supabase/PostgreSQL existente a migraciones compatibles
  con D1;
- reemplazar o adaptar `lib/supabase/**`;
- adaptar servicios que usan `@supabase/supabase-js`;
- revisar variables de entorno y secretos requeridos por Cloudflare;
- validar compatibilidad de Next.js con el runtime de Cloudflare elegido;
- mantener Vercel como plan de contingencia si Cloudflare no permite desplegar
  sin bloquear el proyecto.

## Criterio operativo

Para documentos nuevos:

- No describir Supabase como backend preferido.
- Si se menciona Supabase, aclarar que es estado anterior, legado o fuente para
  migracion de schema.
- Usar Cloudflare D1 como base de datos objetivo.
- Usar Cloudflare como plataforma primaria de deploy.
- Usar Vercel solo como fallback.

## Riesgos

- D1 no es PostgreSQL; la migracion puede requerir ajustar tipos, SQL, relaciones
  y patrones de consulta.
- Las APIs de Supabase client no tienen equivalente directo en D1.
- El despliegue de Next.js en Cloudflare puede requerir adaptadores, cambios de
  runtime o ajustes de build.
- Si se mantiene Vercel como fallback, la capa de datos debe estar disenada para
  no depender accidentalmente de detalles exclusivos de un solo hosting.
