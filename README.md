# RD Environmenty — Sistema web para monitoreo ambiental

[![Demo](https://img.shields.io/badge/demo-rd--environmenty--next.vercel.app-38a169)](https://rd-environmenty-next.vercel.app) [![Panel Admin](https://img.shields.io/badge/admin-vercel.app%2Fadmin-38a169)](https://rd-environmenty-next.vercel.app/admin) [![Arquitectura](https://img.shields.io/badge/docs-arquitectura-8b5cf6)](/digitalceler/rd-environmenty-next-showcase/blob/main/docs/arquitectura.md)

**RD Environmenty** es el sistema web completo de una empresa peruana de servicios ambientales: sitio público con portafolio y blog, panel de administración con CRM de proyectos (monitoreos ambientales en formato OEFA) y portal de clientes con documentos compartidos.

---

## 🧩 ¿Qué incluye?

```
Cliente → Sitio público → Panel admin → CRM (proyectos IMA) → Supabase (PostgreSQL) → Documentos (Cloudflare R2)
```

### Sitio público (`/`)
- Home con hero, Nosotros, Servicios, Portafolio, Blog, Galería de clientes y Contacto (formulario → base de datos).
- Servicios dinámicos: Monitoreos Ambientales, Monitoreo Ocupacional, Biodiversidad y Consultoría.

### Panel admin (`/admin`)
- **CRM IMA**: proyectos como carpetas con código `PM-XXX-2026`, etapas, laboratorios, puntos de monitoreo en formato OEFA (Zona UTM, Datum, Este/Norte, Altitud, Procedencia), subcarpetas de documentos con CRUD, export a Excel.
- Dashboard, Publicaciones, Servicios, Proyectos, Galería, Testimonios, Clientes, Formularios, Documentos, Configuración, Actividad y Usuarios (roles `superadmin` · `admin` · `editor`).

### Portal clientes (`/portal`)
- Login por usuario + contraseña, documentos compartidos (preview/descarga), novedades y gestión de cuenta.

---

## ✨ Características

| Característica | Detalle |
|---|---|
| **CRM de monitoreos** | Proyectos con código editable `PM-XXX-2026`, etapas y estado |
| **Puntos de muestreo** | Formato OEFA: Zona UTM (17/18/19), Datum WGS84/PSAD56, Este/Norte, Altitud, Procedencia; duplicado con código correlativo `EF-01` |
| **Documentos** | Subcarpetas por proyecto con crear/renombrar/eliminar; archivos en Cloudflare R2 (S3-compatible) |
| **Subida masiva** | Drag & drop, progreso/cancelar, descarga ZIP |
| **Portal clientes** | Acceso a documentos y novedades por cliente con login propio |
| **Roles y permisos** | `superadmin`, `admin`, `editor` y `cliente` |
| **Migración de datos** | ETL desde el sistema anterior (PHP/MySQL) hacia Supabase con scripts versionados |
| **Importar desde Drive** | Google Drive Picker integrado en Admin → Documentos |

---

## 🛠️ Stack tecnológico

- **Next.js 15** + **React 19** (App Router, Server Components + Server Actions)
- **TypeScript 5**
- **Tailwind CSS 3** + **Lucide Icons**
- **Supabase** = PostgreSQL + Auth (GoTrue) + Storage
- **Cloudflare R2** (documentos, S3-compatible vía AWS SDK)
- **xlsx** (export a Excel), **Chart.js** (dashboard), **Zod** (validación)
- **Resend** (email opcional)
- **Deploy**: Vercel (producción desde `main`)

---

## 🚀 Demo en vivo

**👉 [rd-environmenty-next.vercel.app](https://rd-environmenty-next.vercel.app)**

- Sitio público: `rd-environmenty-next.vercel.app`
- Panel admin: `rd-environmenty-next.vercel.app/admin`
- Portal clientes: `rd-environmenty-next.vercel.app/portal`

---

## 📸 Capturas de pantalla

### Sitio público
[![Home de RD Environmenty](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/home.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/home.png)

[![Servicios](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/servicios.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/servicios.png)

[![Portafolio](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/portafolio.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/portafolio.png)

[![Blog](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/blog.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/blog.png)

[![Contacto](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/contacto.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/contacto.png)

### Panel admin
[![Login del panel](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/panel-login.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/panel-login.png)

[![CRM de proyectos IMA](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/panel-crm-ima.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/panel-crm-ima.png)

[![Puntos de muestreo formato OEFA](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/panel-puntos-oefa.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/panel-puntos-oefa.png)

[![Subcarpetas de documentos](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/panel-documentos.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/panel-documentos.png)

### Portal clientes
[![Login del portal](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/portal-login.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/portal-login.png)

[![Documentos compartidos](/digitalceler/rd-environmenty-next-showcase/raw/main/assets/screenshots/portal-documentos.png)](/digitalceler/rd-environmenty-next-showcase/blob/main/assets/screenshots/portal-documentos.png)

---

## 🏗️ Arquitectura

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

Detalle completo: [docs/arquitectura.md](/digitalceler/rd-environmenty-next-showcase/blob/main/docs/arquitectura.md)

---

## 🔗 Enlaces

- **Demo**: [rd-environmenty-next.vercel.app](https://rd-environmenty-next.vercel.app)
- **Panel admin**: [rd-environmenty-next.vercel.app/admin](https://rd-environmenty-next.vercel.app/admin)
- **Documentación de arquitectura**: [docs/arquitectura.md](/digitalceler/rd-environmenty-next-showcase/blob/main/docs/arquitectura.md)

---

*Desarrollado por [DigitalCeler](https://digitalceler.com) — Tecnología que impulsa tu negocio.*
