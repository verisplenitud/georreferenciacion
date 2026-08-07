# Instrucciones de configuración — Formulario de Georreferenciación

## Archivos incluidos
- `index.html` — El formulario completo
- `codigo-apps-script.gs` — El script que guarda en Google Sheets
- `INSTRUCCIONES.md` — Este archivo

---

## PASO 1 — Crear la hoja de Google Sheets

1. Abre Google Sheets y crea una hoja nueva
2. Nómbrala como quieras (ej: "Georeferenciacion Colaboradores")
3. Copia el ID de la hoja desde la URL:
   `https://docs.google.com/spreadsheets/d/**ESTE_ES_EL_ID**/edit`

---

## PASO 2 — Configurar el Apps Script

1. En la misma hoja de Sheets, ve a **Extensiones → Apps Script**
2. Borra el código que aparece por defecto
3. Pega todo el contenido del archivo `codigo-apps-script.gs`
4. Reemplaza `PEGA_AQUI_EL_ID_DE_TU_GOOGLE_SHEET` por el ID que copiaste
5. Guarda (Ctrl+S)

### Desplegar el script:
1. Haz clic en **Desplegar → Nueva implementación**
2. En "Tipo", selecciona **Aplicación web**
3. En "Ejecutar como", selecciona **Yo (tu cuenta)**
4. En "Quién tiene acceso", selecciona **Cualquier persona**
5. Haz clic en **Desplegar**
6. Autoriza los permisos que pida Google
7. **Copia la URL** que aparece — la necesitas en el siguiente paso

---

## PASO 3 — Configurar el formulario HTML

Abre `index.html` y edita las dos secciones marcadas con comentarios:

### 3.1 Pega la URL del Apps Script
```javascript
const SCRIPT_URL = 'PEGA_AQUI_LA_URL_DE_TU_APPS_SCRIPT';
// Reemplaza por algo como:
// 'https://script.google.com/macros/s/AKfycb.../exec'
```

### 3.2 Reemplaza los datos de colaboradores
```javascript
const colaboradores = {
  '1712345678': { nombre: 'Juan Pérez', area: 'Ventas', lat: -0.180653, lng: -78.467834, hasGeo: true },
  '1756789012': { nombre: 'Carlos Mora', area: 'Logística', lat: null, lng: null, hasGeo: false }
  // hasGeo: true  → ya tiene coordenadas registradas
  // hasGeo: false → nunca ha registrado (lat y lng van como null)
};
```

Data puede generar esta lista desde la tabla existente con un export a JSON o copiando fila por fila.

---

## PASO 4 — Subir a GitHub Pages

1. Crea un repositorio nuevo en github.com (público)
2. Sube **solo el archivo `index.html`** — los otros son de referencia
3. Ve a **Settings → Pages**
4. En Source selecciona: **Deploy from a branch → main → / (root)**
5. Guarda y espera 1-2 minutos
6. Tu link quedará así:
   `https://tunombredeusuario.github.io/nombre-del-repositorio`

---

## PASO 5 — Verificar que todo funciona

1. Abre el link en tu celular (no en computadora)
2. Ingresa una cédula de prueba
3. Toca "Capturar mi ubicación" y acepta el permiso de GPS
4. Confirma y verifica que aparece una fila nueva en Google Sheets

---

## Estructura de la hoja Sheets (se crea automáticamente)

| Cédula | Nombre | Área | Latitud | Longitud | Acción | Fecha | Hora |
|--------|--------|------|---------|----------|--------|-------|------|
| 1712345678 | Juan Pérez | Ventas | -0.180653 | -78.467834 | Confirmado | 07/08/2026 | 14:32:10 |
| 1756789012 | Carlos Mora | Logística | -0.195012 | -78.489023 | Registrado nuevo | 07/08/2026 | 15:01:44 |

**Valores posibles en columna Acción:**
- `Confirmado` — el punto estaba bien, el colaborador lo confirmó
- `Actualizado` — tenía registro y lo corrigió con GPS
- `Registrado nuevo` — nunca había registrado, es primera vez

---

## Notas importantes

- El GPS **solo funciona con HTTPS** — GitHub Pages lo incluye automáticamente ✓
- Prueba siempre desde el **celular**, no desde computadora
- Si un colaborador no aparece al ingresar su cédula, verifica que esté en el objeto `colaboradores` del HTML
- Para agregar más colaboradores, edita el HTML y vuelve a subir el archivo a GitHub
