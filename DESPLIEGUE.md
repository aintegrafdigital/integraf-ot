# Guía de Despliegue en Vercel

## Opción 1: Despliegue rápido con Vercel CLI ⚡ (Recomendada)

### Paso 1: Instalar Vercel CLI

```bash
npm install -g vercel
```

O si usas Yarn:
```bash
yarn global add vercel
```

### Paso 2: Desplegar

Desde la carpeta del proyecto:

```bash
cd "c:\Users\milto\Aplicaciones Claude\Integraf Ot"
vercel
```

### Paso 3: Seguir las instrucciones

Vercel te preguntará:
- ¿Crear una nueva cuenta o usar una existente? → Responde según corresponda
- ¿Crear nuevo proyecto? → `Y` (Sí)
- ¿Nombre del proyecto? → `integraf-ot` (o el que prefieras)
- ¿En qué directorio está el código? → `.` (punto, significa el actual)
- ¿Desea sobrescribir la configuración? → `N` (No, mantén vercel.json)

### Resultado

Una vez completado, verás algo como:
```
✓ Vercel CLI
✓ Production URL: https://integraf-ot.vercel.app
✓ Inspect: https://vercel.com/tu-usuario/integraf-ot
```

**¡Tu app ya está desplegada!** 🎉

---

## Opción 2: Despliegue con GitHub (Para actualizaciones automáticas)

### Paso 1: Crear repositorio en GitHub

1. Ve a https://github.com/new
2. Nombre del repo: `integraf-ot`
3. Descripción: `Sistema de Órdenes de Trabajo - InteGraf Digital`
4. Privado: ✓ (marca si es interno)
5. Click en "Create repository"

### Paso 2: Conectar repositorio local con GitHub

```bash
cd "c:\Users\milto\Aplicaciones Claude\Integraf Ot"
git init
git add .
git commit -m "Initial commit: Sistema de OT"
git branch -M main
git remote add origin https://github.com/tu-usuario/integraf-ot.git
git push -u origin main
```

### Paso 3: Conectar Vercel con GitHub

1. Ve a https://vercel.com/dashboard
2. Click en "Add New" → "Project"
3. Click en "Import Git Repository"
4. Busca `integraf-ot` en la lista de repositorios
5. Click en "Import"
6. Vercel detectará automáticamente la configuración
7. Click en "Deploy"

### Ventaja: Despliegues automáticos

Cada vez que hagas `git push`, Vercel desplegará automáticamente los cambios. 🚀

---

## Configurar dominio personalizado

Si quieres usar `ot.integrafdigital.cl` en lugar de `integraf-ot.vercel.app`:

### En Vercel:

1. Ve a tu proyecto en vercel.com
2. Configuración → Domains
3. Click en "Add Domain"
4. Ingresa: `ot.integrafdigital.cl`
5. Vercel te mostrará registros DNS

### En tu proveedor de dominio (donde está integrafdigital.cl):

1. Accede a la configuración DNS
2. Añade un registro `CNAME`:
   - **Name:** `ot`
   - **Value:** `cname.vercel-dns.com.`
3. Espera 24-48 horas para propagación

---

## Solucionar problemas

### Error: "CORS error" al guardar OTs

**Causa:** Las credenciales de Supabase podrían tener restricciones de origen.

**Solución:**
1. Ve al panel de Supabase: https://app.supabase.com
2. Proyecto → Authentication → URL Configuration
3. Autoriza la URL de Vercel:
   - Si está en `integraf-ot.vercel.app` → añade esa URL
   - Si está en dominio personalizado → añade `ot.integrafdigital.cl`

### Error: "Unauthorized" al hacer login

**Causa:** Las credenciales de usuario no existen o están incorrectas.

**Solución:**
1. Ve a Supabase: https://app.supabase.com
2. Authentication → Users
3. Click en "Create New User"
4. Email: (ej: `secretaria@integraf.cl`)
5. Password: (establece una temporal, comparte en persona)
6. Confirma el email (aunque no enviaremos correo, es necesario)

### Error: "Could not save OT" después de guardar

**Causa:** Las políticas RLS de Supabase pueden estar mal configuradas.

**Solución:**
Verifica que la tabla `ordenes` tenga RLS habilitado:

1. Supabase → SQL Editor
2. Ejecuta:
```sql
-- Verificar RLS está habilitado
ALTER TABLE ordenes ENABLE ROW LEVEL SECURITY;

-- Verificar políticas existen
SELECT * FROM pg_policies WHERE tablename = 'ordenes';
```

Si no hay políticas, crea una:
```sql
CREATE POLICY "solo autenticados" ON ordenes
  FOR ALL 
  USING (auth.role() = 'authenticated')
  WITH CHECK (auth.role() = 'authenticated');
```

---

## Después del despliegue

### Testear la aplicación

1. Abre https://integraf-ot.vercel.app (o tu dominio)
2. Login con credenciales de usuario
3. Crea una OT de prueba
4. Verifica que se guarde en Supabase
5. Intenta cargarla desde el historial
6. Prueba la impresión (botón "Vista previa")

### Monitoreo

En vercel.com/dashboard puedes ver:
- Estadísticas de visitas
- Errores (Monitoring)
- Logs (Real-time logs)
- Versiones desplegadas (Deployments)

---

## Rollback (volver a versión anterior)

Si algo sale mal:

1. Ve a vercel.com → Tu proyecto → Deployments
2. Busca el despliegue anterior correcto
3. Click en los 3 puntos → "Promote to Production"

¡Listo! Tu app vuelve a la versión anterior en segundos. ⏮️

---

## Preguntas frecuentes

**¿Necesito pagar por Vercel?**
No, tiene plan gratuito generoso. Paga solo si excedes 100 GB ancho de banda/mes.

**¿Cuándo se actualiza después de cambios?**
- Con Vercel CLI: tienes que ejecutar `vercel` nuevamente
- Con GitHub: automáticamente cuando hagas `git push`

**¿Dónde veo los logs?**
En vercel.com → Tu proyecto → Real-time logs (solo últimas horas)

**¿Es seguro poner credenciales Supabase en el código?**
Sí, usamos la `anon key` (pública). RLS en Supabase protege los datos.

---

Cualquier duda, contacta al equipo de desarrollo. 🎯
