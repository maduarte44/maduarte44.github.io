# 🏔️ Nadie Corre Solo — Trail Running App

Aplicación web progresiva (PWA) de planificación y seguimiento para un plan de entrenamiento de trail running de **20 semanas** orientado a completar la Torrencial 44k (44 km · 1.500 m D+) el **27 de junio de 2026**, con dos semanas de recuperación post-carrera incluidas.

---

## 📋 Descripción general

La app es un único archivo HTML autocontenido que no requiere servidor, base de datos ni instalación. Toda la lógica, los estilos, los datos del plan y los assets (incluyendo el splash screen) están embebidos en el archivo. Los datos del usuario se persisten localmente mediante `localStorage` y pueden sincronizarse entre dispositivos vía GitHub Gist.

Está diseñada para instalarse como PWA en Android (Chrome → *Añadir a pantalla de inicio*) y funcionar en modo offline completo gracias a un Service Worker integrado.

---

## 🗂️ Estructura del plan (20 semanas)

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
| 🏁 | **Carrera** | Semana de carrera — 27 jun 2026 |
| 19 | **RECUPERACIÓN POST** | Descanso activo; primer rodaje real el sábado (8 km) |
| 20 | **RECUPERACIÓN POST** | Regreso gradual; largo suave de 8 km el sábado |

**Volumen semanal:** oscila entre 11 km (recuperación post) y 78 km (peak), con una progresión media de ~10% semanal durante la fase de carga.

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
- Header compacto: muestra distancia y desnivel de la carrera, pill de ritmos 🟢/🔴, y botón ↩ cuadrado para cambiar de atleta/carrera.
- Badge de fase activa (BASE, DESARROLLO, PICO…) con color correspondiente.
- Contador regresivo a la carrera en el header.
- Scroll automático al día actual al cargar.
- Al reabrir la app, entra directamente en el último atleta y carrera usados (sin pasar por el selector).

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
- El pill de ritmos en el header muestra los valores en dos líneas compactas (una por ritmo), sin truncarse.

### 📊 Estadísticas
- **Progreso de km:** gráfica de línea con km planificados vs. completados por semana.
- **Tiempo acumulado:** gráfica de barras con minutos de entrenamiento registrados.
- Métricas globales: semanas activas, km totales completados, porcentaje de adherencia.
- Distribución de reacciones emoji.

### 🔤 Nombre de la app
- El título "Nadie Corre Solo" en el header es editable con un solo toque. El nombre personalizado se guarda automáticamente.

---

## ☁️ Sincronización entre dispositivos (GitHub Gist)

Los datos se sincronizan a través de un **Gist privado de GitHub**. No requiere servidor propio. El botón ☁️ está en la barra de navegación inferior.

### Configuración inicial (dispositivo principal)

1. Ve a [github.com/settings/tokens/new](https://github.com/settings/tokens/new).
2. Dale un nombre (ej: *nadie-corre-solo*), selecciona **solo el scope `gist`**, y genera el token.
3. En la app, toca el botón **☁️** en la barra inferior.
4. Pega el token y pulsa **⬆ Subir** — la app crea automáticamente un Gist privado llamado `nadie-corre-solo-backup.json`.

### Conectar un segundo dispositivo

1. Introduce el mismo token en el nuevo dispositivo.
2. Necesitas también el **Gist ID** — cópialo desde el primer dispositivo (aparece en el modal como `Gist: xxxxxxxx…`).
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

El botón ☁️ muestra un punto amarillo cuando hay cambios pendientes de subir, y verde tras sincronizar correctamente.

### Seguridad

- El token se guarda únicamente en `localStorage` del dispositivo, nunca pasa por un servidor intermedio.
- Las llamadas van directamente a `api.github.com` desde el navegador.
- El Gist es **privado** y el token tiene el scope mínimo posible (`gist`).

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
| `tw_last_aid` / `tw_last_rid` | Último atleta y carrera usados (auto-inicio) |
| `tw_sync_pat` | Personal Access Token de GitHub (solo local) |
| `tw_sync_gist_id` | ID del Gist vinculado |

---

## 📱 Instalación como PWA (Android)

1. Abre la URL del hosting en **Chrome para Android**.
2. Toca el menú ⋮ → **"Añadir a pantalla de inicio"**.
3. La app se instala con ícono propio, modo standalone (sin barra de navegación) y funciona sin conexión.

> El Service Worker hace cache de todos los recursos en la primera carga. Las sesiones posteriores funcionan completamente offline.

---

## 🌐 Despliegue en GitHub Pages

```bash
# Clonar el repositorio
git clone https://github.com/maduarte44/maduarte44.github.io.git
cd maduarte44.github.io

# Copiar archivos actualizados
cp ~/Downloads/index.html .
cp ~/Downloads/README.md .

# Publicar
git add index.html README.md
git commit -m "descripción del cambio"
git push
```

La app queda disponible en `https://maduarte44.github.io` en 1–2 minutos.

> ⚠️ El Service Worker requiere **HTTPS** o `localhost`. Un archivo abierto como `file://` no activará el modo offline ni el manifest PWA.

---

## 🛠️ Stack técnico

| Componente | Tecnología |
|---|---|
| Estructura | HTML5, un solo archivo autocontenido |
| Estilos | CSS custom properties, sin frameworks |
| Lógica | Vanilla JavaScript (ES2020+) |
| Gráficas | Chart.js 4.4 (CDN) |
| Fuentes | Inter (UI principal) + JetBrains Mono (datos numéricos) |
| Offline | Service Worker con cache-first strategy |
| Instalación | Web App Manifest generado dinámicamente |
| Persistencia | localStorage + GitHub Gist (sync opcional) |
| Splash | Imagen JPEG embebida en base64, recortada sin bordes blancos |
| Zona horaria | America/Santiago (Chile continental) |

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
    weeks: buildWeeks()        // Función que construye las semanas
  }]
}];
```

Las semanas se construyen con dos helpers:
- `W(id, fecha, label, sesión, tipo, km, descripción)` — sesión de running
- `F(id, fecha, label, series, descripción, [ejercicios])` — sesión de fuerza

---

## 📁 Archivos del proyecto

```
/
├── index.html          # App completa (único archivo necesario para producción)
└── README.md           # Este archivo
```

---

## 📄 Licencia

Uso personal. Sin licencia de distribución abierta.
