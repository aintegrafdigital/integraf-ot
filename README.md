# Sistema de Órdenes de Trabajo — InteGraf Digital

Aplicación web para gestionar órdenes de producción de la imprenta InteGraf Digital, con autenticación por usuario y almacenamiento en Supabase.

## Características

- ✅ Autenticación con email y contraseña (Supabase Auth)
- ✅ Formulario completo de Órdenes de Trabajo (OT)
- ✅ Ítems con terminaciones individuales
- ✅ Vista previa e impresión en formato A4
- ✅ Historial de OTs con búsqueda y carga
- ✅ Guardado automático (upsert por N° de OT)
- ✅ Eliminación de OTs desde el historial
- ✅ Sesión persistente (sessionStorage)

## Stack tecnológico

- **Frontend:** HTML5 + CSS3 + JavaScript vanilla
- **Base de datos:** Supabase (PostgreSQL)
- **Autenticación:** Supabase Auth
- **Hosting:** Vercel

## Desarrollo local

### Requisitos
- Python 3.x (para servidor local)

### Ejecutar localmente

```bash
# En Windows (PowerShell)
python -m http.server 3000

# O en macOS/Linux
python3 -m http.server 3000
```

Luego abre: http://localhost:3000

### Variables de entorno

Las credenciales de Supabase están configuradas en `index.html` (líneas ~630-631):
- `SUPA_URL`
- `SUPA_KEY`

Para cambiarlas, edita directamente en el archivo HTML.

## Despliegue en Vercel

### Opción A: Con GitHub (recomendada)

1. Crear repositorio en GitHub:
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/tu-usuario/integraf-ot.git
git push -u origin main
```

2. En vercel.com:
   - Click en "Add New" → "Project"
   - Conectar GitHub
   - Seleccionar este repositorio
   - Click "Deploy"

### Opción B: Con Vercel CLI

```bash
npm install -g vercel
vercel
# Seguir las instrucciones interactivas
```

## Estructura del proyecto

```
integraf-ot/
├── index.html          ← Aplicación completa (HTML + CSS + JS)
├── vercel.json         ← Configuración de Vercel
├── package.json        ← Metadatos del proyecto
├── .gitignore          ← Archivos a ignorar en Git
└── README.md           ← Este archivo
```

## Notas de seguridad

- Las credenciales de Supabase son públicas (anon key) — esto es normal
- Los usuarios se crean manualmente desde el panel de Supabase
- Row Level Security (RLS) protege los datos por autenticación
- No se mandan correos de invitación — el admin crea el usuario y comparte credenciales en persona

## Mejoras futuras

- [ ] Búsqueda y filtros avanzados en historial
- [ ] Cambio de contraseña desde la app
- [ ] Panel de administrador
- [ ] Exportación a Excel
- [ ] Dominio personalizado: `ot.integrafdigital.cl`

## Contacto

Para soporte o cambios, contactar al área comercial de InteGraf Digital.
