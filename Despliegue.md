# 🚀 Despliegue.md — Proceso de Despliegue en Azure Static Web Apps

**Autor:** Luis Rodriguez Zabala  
**Fecha:** Abril 2026  
**Plataforma:** Microsoft Azure Static Web Apps  
**URL desplegada:** https://black-sky-0eac8bf10.7.azurestaticapps.net/

---

## 📋 Resumen del Proceso

| Fase | Descripción | Estado |
|------|------------|--------|
| 1 | Instalación de Node.js | ✅ Completado |
| 2 | Compilación del proyecto Angular | ✅ Completado |
| 3 | Creación del repositorio en GitHub | ✅ Completado |
| 4 | Subida del código compilado | ✅ Completado |
| 5 | Creación de Azure Static Web App | ✅ Completado |
| 6 | Configuración de headers de seguridad | ✅ Completado |
| 7 | Corrección de calificación C a A | ✅ Completado |
| 8 | Verificación en securityheaders.com | ✅ Completado — Calificación: A |

---

## ⚙️ Fase 1 — Instalación de Node.js

### ❌ Error — npm no reconocido como comando

Al intentar ejecutar `npm install` en CMD apareció el siguiente error:

> *"npm no se reconoce como un comando interno o externo"*

**Causa:** Node.js no estaba instalado en el sistema.

**Solución:**
1. Descargar Node.js LTS desde https://nodejs.org
2. Ejecutar el instalador `.msi` asegurando que **"Add to PATH"** esté marcado
3. Cerrar CMD y abrir uno nuevo
4. Verificar instalación:

```bash
node --version
npm --version
```

✅ Instalación exitosa con Node.js v18+

---

## ⚙️ Fase 2 — Compilación del Proyecto Angular

### Comandos ejecutados

**1. Navegar a la carpeta del proyecto:**
```bash
cd C:\pokedex\pokedex-angular
```

**2. Instalar dependencias:**
```bash
npm install
```

**Resultado:**
```
added 1224 packages, and audited 1225 packages in 38s
```

**3. Compilar para producción:**
```bash
npm run build
```

**Resultado:** Se generó exitosamente la carpeta:
```
C:\pokedex\pokedex-angular\dist\pokedex-angular\
```

---

## 📁 Fase 3 — Creación del Repositorio en GitHub

1. Ingresar a https://github.com y crear nuevo repositorio
2. Configuración usada:
   - **Nombre:** `pokedex`
   - **Visibilidad:** Public
   - **Initialize with README:** ✅ Sí
   - **.gitignore:** None
   - **License:** None
3. Repositorio creado exitosamente

---

## 📤 Fase 4 — Subida del Código a GitHub

Se utilizó **GitHub Desktop** para subir todos los archivos sin restricción de cantidad.

### Pasos con GitHub Desktop

**1. Clonar el repositorio:**
- File → Clone a repository
- Seleccionar `pokedex`
- Local path: `C:\pokedex-repo\`

**2. Copiar archivos compilados:**
- Origen: `C:\pokedex\pokedex-angular\dist\pokedex-angular\`
- Destino: `C:\pokedex-repo\`
- Seleccionar todo `CTRL+A` → Copiar `CTRL+C` → Pegar `CTRL+V`

**3. Hacer commit y push:**
- Summary: `Add compiled PokeDex Angular app`
- Clic en **"Commit to main"**
- Clic en **"Push origin"**

✅ Archivos subidos correctamente al repositorio.

---

## ☁️ Fase 5 — Creación de Azure Static Web App

### Configuración del recurso

| Campo | Valor usado |
|-------|------------|
| Subscription | Azure for Students |
| Resource Group | `rg-pokedex` (nuevo) |
| Name | `pokedex-app` |
| Plan type | Free |
| Region | West US 2 |
| Source | GitHub |
| Repository | `pokedex` |
| Branch | `main` |
| Build Preset | Custom |
| App location | `/` |
| Output location | `/` |

Azure creó automáticamente un **GitHub Action** que despliega la aplicación en cada push a `main`. El workflow mostró estado ✅ verde al completarse.

**URL pública asignada:** https://black-sky-0eac8bf10.7.azurestaticapps.net/

---

## 🔒 Fase 6 — Configuración de Headers de Seguridad

Se creó el archivo `staticwebapp.config.json` directamente en GitHub mediante **"Add file"** → **"Create new file"**.

### ❌ Error — Calificación C en securityheaders.com

Al escanear la app por primera vez en securityheaders.com, la calificación fue **C** con los siguientes headers en rojo:
- ❌ `Content-Security-Policy`
- ❌ `X-Frame-Options`
- ❌ `Permissions-Policy`

**Causa:** El archivo `staticwebapp.config.json` tenía un error de formato que impedía que Azure aplicara los headers correctamente.

**Solución:**
1. Ir al repo en GitHub → clic en `staticwebapp.config.json`
2. Clic en el ícono de basurero 🗑️ → Commit para eliminar el archivo
3. Crear el archivo nuevamente con **"Add file"** → **"Create new file"**
4. Pegar el contenido correcto y hacer commit
5. Esperar el Action verde ✅
6. Volver a escanear → calificación subió a **A** ✅

### Configuración final funcionando

```json
{
  "globalHeaders": {
    "Content-Security-Policy": "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; img-src 'self' data: blob: https://raw.githubusercontent.com https://pokeapi.co https://assets.pokemon.com; connect-src 'self' https://pokeapi.co https://beta.pokeapi.co; font-src 'self' https://fonts.gstatic.com; object-src 'none'; base-uri 'self'; form-action 'self';",
    "Strict-Transport-Security": "max-age=31536000; includeSubDomains; preload",
    "X-Content-Type-Options": "nosniff",
    "X-Frame-Options": "DENY",
    "Referrer-Policy": "no-referrer",
    "Permissions-Policy": "geolocation=(), microphone=(), camera=()"
  },
  "navigationFallback": {
    "rewrite": "/index.html"
  }
}
```

---

## ✅ Fase 7 — Verificación Final

### Prueba en securityheaders.com

- **URL escaneada:** https://black-sky-0eac8bf10.7.azurestaticapps.net/
- **Calificación obtenida:** 🟢 **A**
- **Headers verificados:**
  - ✅ Content-Security-Policy
  - ✅ Strict-Transport-Security
  - ✅ X-Content-Type-Options
  - ✅ X-Frame-Options
  - ✅ Referrer-Policy
  - ✅ Permissions-Policy

### Validaciones finales

| Validación | Resultado |
|-----------|-----------|
| App carga correctamente desde URL pública | ✅ |
| Imágenes de Pokémon visibles | ✅ |
| HTTPS activo | ✅ |
| Sin errores 404/500 | ✅ |
| Calificación en securityheaders.com | ✅ A |

---

## 📎 Herramientas Utilizadas

| Herramienta | Uso |
|------------|-----|
| Node.js + npm | Compilación del proyecto Angular |
| GitHub Desktop | Subida masiva de archivos al repositorio |
| Azure Portal | Creación y configuración del Static Web App |
| GitHub Actions | CI/CD automatizado |
| securityheaders.com | Verificación de headers de seguridad |
