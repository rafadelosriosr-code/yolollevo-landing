# Yo Lo Llevo — Landing + Panel Admin

Landing page con panel de administración visual (Decap CMS) para editar **todo el contenido** sin tocar código.

---

## 📁 Estructura

```
yolollevo-site/
├── index.html         ← Landing (estructura + diseño)
├── content.json       ← Contenido editable (lo modifica el panel)
├── netlify.toml       ← Config de hosting
└── admin/
    ├── index.html     ← Panel de administración (/admin)
    └── config.yml     ← Define los campos editables del panel
```

---

## 🚀 Deploy en Netlify (paso a paso)

### 1. Subir a GitHub

1. Creá un repositorio nuevo en GitHub (ej: `yolollevo-landing`).
2. Subí los archivos de esta carpeta a ese repo.

### 2. Conectar a Netlify

1. Andá a [app.netlify.com](https://app.netlify.com) y registrate (es gratis).
2. Click en **"Add new site"** → **"Import an existing project"**.
3. Conectá tu cuenta de GitHub y elegí el repo `yolollevo-landing`.
4. Dejá la config por defecto (Netlify detecta `netlify.toml` solo).
5. Click en **"Deploy site"**.

En ~1 minuto tu landing está online en una URL tipo `https://random-name-123.netlify.app`.

### 3. Activar Identity (para el login del panel)

El panel admin necesita un sistema de login. Netlify Identity es gratuito.

1. En tu sitio de Netlify, andá a **Site configuration** → **Identity**.
2. Click en **"Enable Identity"**.
3. En **Registration**, elegí **"Invite only"** (para que solo vos puedas entrar).
4. Scroll hasta **Services** → **Git Gateway** → **"Enable Git Gateway"**.

### 4. Invitarte a vos mismo

1. En la pestaña **Identity** de tu sitio, click en **"Invite users"**.
2. Ingresá tu email y enviá la invitación.
3. Vas a recibir un email con un link → click → creás tu contraseña.

### 5. Agregar el widget de Identity al admin

Editá `admin/index.html` y agregá esta línea antes del `</body>`:

```html
<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
```

Y editá `index.html` agregando en el `<head>`:

```html
<script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
```

(El paso 5 ya está hecho en estos archivos.)

---

## ✏️ Cómo editar el contenido

1. Andá a `https://tu-sitio.netlify.app/admin/`
2. Login con el email/contraseña que creaste.
3. Editás los campos visualmente.
4. Click en **"Publish"** → en ~1 min los cambios están live.

### Qué se puede editar desde el panel

- ✅ Google Analytics (Measurement ID + switch on/off)
- ✅ Número de WhatsApp y mensajes pre-llenados (por sección)
- ✅ Todos los textos del Hero (eyebrow, título, subtítulo, CTAs, stats)
- ✅ Sección Problema (título, subtítulo, lista de puntos)
- ✅ Precio (monto, moneda, qué incluye, CTA)
- ✅ Testimonios (agregar/editar/eliminar)
- ✅ FAQ (agregar/editar/eliminar preguntas)
- ✅ CTA Final
- ✅ Footer (descripción, redes, copyright)

---

## 📊 Google Analytics 4

### Cómo activarlo

1. Andá a [analytics.google.com](https://analytics.google.com) y creá una propiedad GA4 (gratis).
2. En **Admin → Data Streams → Web**, agregá tu dominio.
3. Copiá el **Measurement ID** (formato `G-XXXXXXXXXX`).
4. En tu panel admin: sección **📊 Google Analytics**:
   - Pegá el ID
   - Activá el switch
   - Click en **Publish**

### Qué se trackea automáticamente

| Evento | Cuándo se dispara | Parámetros |
|---|---|---|
| `page_view` | Cada carga de página | (estándar GA4) |
| `whatsapp_click` | Click en cualquier botón WhatsApp | `source`: hero / nav / precios / cta_final / footer / float |
| `faq_open` | Cuando se abre una pregunta del FAQ | `pregunta`: texto de la pregunta |
| `cta_secundario_click` | Click en "Ver cómo funciona" | — |
| `scroll_depth` | Al alcanzar 25%, 50%, 75%, 100% del scroll | `percent`: 25/50/75/100 |

### Cómo usar estos datos en GA4

En **Reports → Engagement → Events** vas a ver todos los eventos listados.

**Tip importante:** Marcá `whatsapp_click` como **conversión** en GA4 (Admin → Events → toggle "Mark as conversion"). Así podés:
- Ver cuántas conversiones tenés por día
- Saber qué fuente de tráfico convierte mejor (Google Ads, Instagram, directo)
- Comparar performance entre los distintos botones por el parámetro `source`

### Pausar el tracking temporalmente

En el panel, apagá el switch **"Activar Google Analytics"** y publicá. El ID queda guardado pero no se carga el script.

---

### Qué NO se edita desde el panel (queda en el HTML)

- Estructura visual / colores / fuentes
- La sección "Cómo funciona" (timeline de 6 pasos)
- La sección "Solución" (6 bullets con números)
- Iconos SVG

Si necesitás cambiar algo de esto, lo hacemos juntos en otra iteración.

---

## 🧪 Probar localmente (opcional)

Si querés probar el panel antes de subirlo:

```bash
# 1. Instalar el servidor local de Decap (necesita Node.js)
npx decap-server

# 2. En otra terminal, servir el sitio
npx serve .

# 3. Descomentá esta línea en admin/config.yml:
# local_backend: true

# 4. Abrir http://localhost:3000/admin/
```

---

## 🆘 Si algo falla

- **El panel no abre**: chequear que Identity y Git Gateway estén activados.
- **No puedo loguearme**: revisar el email de invitación, o reenviarlo desde Netlify.
- **Cambios no aparecen**: esperar 1 minuto (Netlify rebuildea), o forzar refresh con Ctrl+F5.
- **content.json se rompió**: en GitHub, podés ver el historial y revertir al commit anterior.
