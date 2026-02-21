# 🏔️ Nadie Corre Solo — Trail Training App

Aplicación web progresiva (PWA) de planificación y seguimiento para un plan de entrenamiento de trail running de 18 semanas orientado a completar una carrera de 44 km con 1.500 m D+ el **27 de junio de 2026**.

---

## 📋 Descripción general

La app es un único archivo HTML autocontenido que no requiere servidor, base de datos ni instalación. Toda la lógica, los estilos, los datos del plan y los assets (incluyendo el splash screen) están embebidos en el archivo. Los datos del usuario se persisten localmente mediante `localStorage`.

Está diseñada para instalarse como PWA en Android (Chrome → *Añadir a pantalla de inicio*) y funcionar en modo offline completo gracias a un Service Worker integrado.

---

## 🗂️ Estructura del plan

| Semanas | Fase | Descripción |
|---|---|---|
| 1–3 | **BASE** | Construcción de base aeróbica, técnica y hábito de entrenamiento |
| 4 | **RECUPERACIÓN** | Semana de asimilación y descanso activo |
| 5–7 | **DESARROLLO** | Incremento progresivo de volumen e intensidad |
| 8 | **RECUPERACIÓN** | Segunda semana de asimilación |
| 9–11 | **PICO** | Cargas máximas, largo de 30–35 km |
| 12 | **RECUPERACIÓN** | Recuperación activa pre-peak |
| 13–15 | **PICO / PEAK 🔺** | Semanas de máximo volumen; largo pico de 35 km |
| 16–17 | **TAPER** | Reducción progresiva de carga |
| 18 | **TAPER FINAL** | Última semana, preparación mental y logística |
| 🏁 | **CARRERA** | Semana de carrera — 27 jun 2026 |

**Volumen semanal:** oscila entre 17 km (taper final) y 78 km (peak), con una progresión media de ~10% semanal.

---

## 🏋️ Tipos de entrenamiento

Cada sesión tiene un tipo visual codificado por color:

| Tipo | Color | Descripción |
|---|---|---|
| 🟢 **SUAVE** | Verde | Rodajes fáciles, recuperación activa, ritmo conversacional (~6:40/km) |
| 🔴 **INTENSO** | Naranja-rojo | Intervalos, repeticiones, largos de trail con desnivel |
| 💪 **FUERZA** | Azul-violeta | Sesiones de tren inferior, core y propiocepción |
| ⬛ **DESCANSO** | Gris oscuro | Descanso completo o movilidad libre |

---

## ✨ Funcionalidades

### 📅 Calendario semanal
- Navegación semana a semana con flechas o **swipe horizontal**.
- Badge de fase activa (BASE, DESARROLLO, PICO…) con color correspondiente.
- Contador regresivo a la carrera en el header.
- Scroll automático al día actual al cargar.

### 📝 Modal de entrenamiento
Al tocar una tarjeta se abre el detalle con:
- Descripción de la sesión y kilometraje planificado.
- **Registro real:** distancia completada, tiempo y ritmo calculado en tiempo real.
- **Reacción emoji** al entrenamiento (💪 🔥 😴 🤕).
- Para sesiones de fuerza: lista de ejercicios con descripciones técnicas expandibles.

### ↕️ Modo intercambio (Swap)
- **Mantén pulsado** (~500 ms) una tarjeta para activar el modo intercambio.
- Un banner amarillo indica que el modo está activo.
- Toca otra tarjeta para intercambiar ambas sesiones dentro de la semana.
- El intercambio es único: el modo se desactiva automáticamente tras cada swap.
- Toca la tarjeta seleccionada para cancelar sin hacer cambios.

### ⏱️ Perfil de ritmos
- Configura tu **ritmo suave** (🟢) y **ritmo intenso** (🔴) en min/km.
- La app estima la duración de cada sesión según el tipo.
- Al guardar un entrenamiento real, el sistema aprende y ajusta tus ritmos automáticamente usando un promedio ponderado con el historial.
- El pill de ritmos en el header muestra tus valores actuales en todo momento.

### 📊 Estadísticas
- **Progreso de km:** gráfica de línea con km planificados vs. completados por semana.
- **Tiempo acumulado:** gráfica de barras con minutos de entrenamiento registrados.
- Métricas globales: semanas activas, km totales completados, porcentaje de adherencia.
- Distribución de reacciones emoji.

### 🔤 Nombre de la app
- El título "Nadie Corre Solo" en el header es editable con un solo toque. El nombre personalizado se guarda automáticamente.

---

## 💾 Almacenamiento de datos

Toda la persistencia utiliza `localStorage` con claves con namespace por atleta y carrera:

| Clave | Contenido |
|---|---|
| `tw_weeks_{athleteId}_{raceId}` | Semanas con posibles intercambios aplicados |
| `tw_rxn_{athleteId}_{raceId}` | Reacciones emoji por sesión |
| `tw_logs_{athleteId}_{raceId}` | Registros reales (distancia, tiempo) |
| `tw_paces_{athleteId}_{raceId}` | Perfil de ritmos (suave e intenso) |
| `tw_ph_{key}_{athleteId}_{raceId}` | Historial de ritmos para auto-aprendizaje |
| `tw_title_{athleteId}_{raceId}` | Nombre personalizado de la app |

> ⚠️ Los datos residen únicamente en el dispositivo. No hay sincronización en la nube. Para hacer backup, exporta los datos de `localStorage` manualmente o duplica el dispositivo.

---

## 📱 Instalación como PWA (Android)

1. Abre `index.html` desde tu hosting en **Chrome para Android**.
2. Toca el menú ⋮ → **"Añadir a pantalla de inicio"**.
3. La app se instala con ícono propio, modo standalone (sin barra de navegación) y funciona sin conexión.

> El Service Worker hace cache de todos los recursos en la primera carga. Las sesiones posteriores funcionan completamente offline.

---

## 🌐 Despliegue

La app es un único archivo estático. Cualquier hosting de archivos estáticos funciona:

### Netlify (recomendado)
```
# Opción 1 — Drag & Drop
Arrastra index.html a app.netlify.com/drop

# Opción 2 — CLI
npm install -g netlify-cli
netlify deploy --prod --dir . --message "deploy"
```

### GitHub Pages
```bash
git init
git add index.html README.md
git commit -m "initial"
git branch -M main
git remote add origin https://github.com/tu-usuario/tu-repo.git
git push -u origin main
# Activa Pages en Settings → Pages → Source: main / root
```

### Servidor local (desarrollo)
```bash
# Python
python3 -m http.server 8080

# Node
npx serve .
```

> ⚠️ El Service Worker requiere **HTTPS** o `localhost` para registrarse. Un archivo abierto directamente como `file://` no activará el modo offline ni el manifest de PWA.

---

## 🛠️ Stack técnico

| Componente | Tecnología |
|---|---|
| Estructura | HTML5 semántico, un solo archivo |
| Estilos | CSS custom properties, sin frameworks |
| Lógica | Vanilla JavaScript (ES2020+) |
| Gráficas | Chart.js 4.4 (CDN) |
| Fuentes | Inter (UI principal) + JetBrains Mono (datos numéricos) |
| Offline | Service Worker con cache-first strategy |
| Instalación | Web App Manifest generado dinámicamente |
| Persistencia | localStorage (sin backend) |
| Splash | Imagen JPEG embebida en base64 |

---

## 📁 Archivos del proyecto

```
/
├── index.html          # App completa (único archivo necesario para producción)
└── README.md           # Este archivo
```

---

## 🔧 Personalización

Para adaptar la app a otro atleta o carrera, editar el objeto `ATHLETES` en el JavaScript embebido:

```javascript
const ATHLETES = [{
  id: 'nombre-atleta',
  name: 'Nombre Atleta',
  races: [{
    id: 'nombre-carrera-2026',
    name: 'Nombre de la Carrera',
    date: '2026-06-27',        // Fecha de carrera (YYYY-MM-DD)
    km: 44,                    // Distancia en km
    dplus: 1500,               // Desnivel positivo en metros
    weeks: buildWeeks()        // Función que construye las 18 semanas
  }]
}];
```

Las semanas se construyen con dos helpers:
- `W(id, fecha, label, sesión, tipo, km, descripción)` — sesión de running
- `F(id, fecha, label, series, descripción, [ejercicios])` — sesión de fuerza

---

## 📄 Licencia

Uso personal. Sin licencia de distribución abierta.

---

## ☁️ Sincronización entre dispositivos (GitHub Gist)

Los datos se sincronizan a través de un **Gist privado de GitHub**. No requiere servidor propio.

### Configuración inicial (dispositivo principal)

1. Ve a [github.com/settings/tokens/new](https://github.com/settings/tokens/new).
2. Dale un nombre (ej: *nadie-corre-solo*), selecciona **solo el scope `gist`**, y genera el token.
3. En la app, toca el botón **☁️** en el header.
4. Pega el token y pulsa **⬆ Subir** — la app crea automáticamente un Gist privado llamado `nadie-corre-solo-backup.json`.

### Conectar un segundo dispositivo

1. Genera el mismo token (o reutiliza el mismo) en el nuevo dispositivo.
2. Además del token, necesitas el **Gist ID** — cópialo desde el primer dispositivo (aparece en el modal de sync como `Gist: xxxxxxxx…`).
3. Guarda el Gist ID en `localStorage` del nuevo dispositivo ejecutando en la consola del navegador:
   ```js
   localStorage.setItem('tw_sync_gist_id', 'TU_GIST_ID_COMPLETO')
   ```
4. Toca **☁️ → ⬇ Bajar** para traer todos los datos.

### Flujo de uso diario

| Acción | Cuándo |
|---|---|
| **⬆ Subir** | Después de registrar entrenamientos o hacer cambios |
| **⬇ Bajar** | Al abrir la app en un dispositivo que no fue el último en editar |

El botón ☁️ muestra un punto amarillo (🟡) cuando hay cambios locales pendientes de subir, y se vuelve verde (✓) tras sincronizar correctamente.

### Estrategia de merge

Se usa **last-write-wins** a nivel de snapshot completo: el conjunto de datos con el timestamp más reciente prevalece. Evita editar desde dos dispositivos sin conexión simultáneamente.

### Seguridad

- El token se guarda únicamente en `localStorage` del dispositivo, nunca pasa por un servidor intermedio.
- Las llamadas van directamente a `api.github.com` desde el navegador.
- El Gist es **privado** — no visible públicamente aunque tengas la URL.
- Usa el mínimo scope posible (`gist`): el token no tiene acceso a tus repositorios ni a ningún otro recurso.
