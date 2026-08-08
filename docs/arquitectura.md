# Arquitectura de RD Environmenty

## Diagrama

```
┌──────────────┐       ┌──────────────┐       ┌──────────────┐
│   Usuario     │──────▶│  Next.js 15  │──────▶│  Supabase    │
│ (Web/Admin)   │       │   (Vercel)   │       │ PostgreSQL   │
└──────────────┘       └──────────────┘       │  + Auth      │
                                              └──────┬───────┘
                                                     │
                                            ┌────────▼─────────┐
                                            │  Cloudflare R2   │
                                            │ (documentos S3)  │
                                            └──────────────────┘
```

## Componentes

| Componente | Tecnología | Función |
|---|---|---|
| **Sitio público** | Next.js 15 + TypeScript + Tailwind CSS | Home, Nosotros, Servicios, Portafolio, Blog, Galería, Contacto |
| **Panel admin** | Next.js 15 (Server Actions) | CRUD de módulos + CRM de proyectos de monitoreo |
| **Portal clientes** | Next.js 15 | Login por usuario + contraseña, documentos y novedades |
| **Base de datos** | Supabase (PostgreSQL) | Datos de todos los módulos y del CRM IMA |
| **Auth** | GoTrue (Supabase Auth) | Sesiones de admin (roles) y de portal (clientes) |
| **Documentos** | Cloudflare R2 (S3-compatible, presigned PUT) | Archivos de carpetas y subcarpetas por proyecto |
| **Storage imágenes** | Supabase Storage | Logos y galería |
| **Email** | Resend | Emails transaccionales (opcional) |
| **Importar** | Google Drive Picker | Importación de archivos en Admin → Documentos |

## CRM IMA (monitoreos ambientales)

Flujo del módulo de proyectos:

```
Crear proyecto → Carpeta con código PM-XXX-2026
       │
       ├── Datos generales (cliente, título, periodo, fecha, ubicación)
       ├── Laboratorio y etapas (preliminar + avances)
       ├── Puntos de muestreo → formato OEFA
       │     · Código correlativo EF-01, EF-02…
       │     · Nombre del punto, Zona UTM (17/18/19), Datum (WGS84/PSAD56)
       │     · Este / Norte, Altitud (m.s.n.m.), Procedencia
       │     · Duplicar punto con datos copiados
       └── Documentos → subcarpetas (crear/renombrar/eliminar)
             · Archivos en Cloudflare R2, export a Excel de puntos
```

## Seguridad y acceso

- Server Actions protegidas: solo usuarios con rol admin (`superadmin` / `admin` / `editor`).
- El portal de clientes solo accede a sus propios documentos.
- Las claves de servicio (Supabase service role, R2) viven solo en el servidor, nunca en el cliente.
- Middleware de Next.js valida sesiones en `/admin` y `/portal`.

## Despliegue

- **Web**: Vercel (deploy automático desde la rama `main`).
- **Base de datos + Auth + Storage**: Supabase.
- **Documentos**: Cloudflare R2.
- **Migración de datos**: scripts ETL (`tsx`) desde el sistema anterior PHP/MySQL hacia Supabase, con SQL versionado en `supabase/migrations/`.
