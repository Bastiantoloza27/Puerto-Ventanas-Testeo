# Handoff: Testeos Aleatorios — Puerto Ventanas S.A.

Plataforma para la Política de Alcohol y Drogas de PVSA. Hace un sorteo justo y auditable de los trabajadores a testear, solo entre quienes están en turno. Indica dónde está cada persona para que el equipo de Prevención la ubique en terreno y deja trazabilidad completa: actas, bitácora y seguimiento.

## Sobre estos archivos
`prototipo.dc.html` es una **referencia de diseño en HTML** (prototipo navegable), **no código de producción**. La tarea es **recrearlo en el stack del builder** (recomendado: React + TypeScript, BD relacional, auth por roles, almacenamiento de archivos y envío de correo). Se abre en un navegador con `support.js` al lado.

## Fidelidad
**Alta fidelidad**: colores, tipografía, espaciado e interacciones finales. `referencias/wireframes.dc.html` es baja fidelidad (exploración de flujos).

## Design tokens (compartidos)
- **Teal marca (logo)** `#00ABAC` · **Teal UI (botones/texto, AA)** `#007F80` · hover/texto teal `#005F60` · teal suave `#E2F4F4` / `#E8F5F5`
- **Sidebar / superficies oscuras** `#0E2A2B` · activo `#1B4A4A` · borde oscuro `#1F4344` · texto sidebar `#A9C1C0` / etiquetas `#6F9493` / avatar texto `#7FD6D6`
- **Fondo app** `#EDF2F1` con degradado superior `linear-gradient(180deg,#DCECEA 0,#EDF2F1 260px)`
- **Texto** `#1C2526` · secundario `#3C4849` · terciario `#6B7778` · bordes `#DDE5E4` / `#E3E7E7` / divisores `#EEF1F1` / inputs `#CDD4D4`
- **Ámbar (pendiente/aviso)** fondo `#FBF3DC` / `#FBFAF6`, texto `#7A5D0E`, barra `#C49A1C`, borde `#C9B98A`
- **Rojo (no ejecutado / no negativo)** `#9B3B2E`, botón `#B9574A`, fondo `#F6E4E0`
- **Tipografía**: IBM Plex Sans (UI 13–15px, 400/500/600), IBM Plex Mono (códigos, horas, cifras; eyebrows 12px +0.08em), Newsreader (títulos de pantalla 34px/1.1, títulos de tarjeta 20–24px)
- **Tarjetas**: fondo `#FFF`, borde 1px `#DDE5E4`, radio 10px, sombra `0 1px 2px rgba(14,42,43,.05), 0 6px 18px rgba(14,42,43,.04)`. KPIs con `border-top: 3px solid #00ABAC`.
- **Chips**: radio 14px, 5–7px × 11–12px; seleccionado = fondo y borde `#007F80`, texto blanco.
- **Layout**: sidebar fijo 232px (sticky, 100vh) + contenido fluido (padding 28px 32px), columnas con `flex-wrap` y panel lateral derecho 280–400px.


## Funcionalidad

### Insumos y fuentes de verdad
- **Nombrada** (PDF diario, ver `insumos-ejemplo/`): faena → nave → sitio → código, nombre, calificación; columnas ROL_A (1° turno 08:00–15:30) y ROL_B (2° turno 15:30–23:00). Cambios llegan por WhatsApp → nueva versión del archivo.
- **Rotativa de terminales** (mensual, Excel): define el 3° turno (23:00–08:00) y terminales.
- **Listado administrativo** (horario fijo). **Contratistas** y **Fepasa** (pendientes de definir).
- **Maestro de personas por código**: cruce de todas las fuentes; nunca por nombre. Si nombrada y rotativa discrepan, manda la nombrada y queda aviso.

### Pantallas
1. **Splash** (3,5 s, clic para saltar, respeta `prefers-reduced-motion`): fondo `#0E2A2B`, logo con revelado `clip-path` izq→der (1,2s, delay .2s), línea teal que se dibuja (1,8s), título Newsreader + eyebrow mono, leyenda "Uso interno · Información confidencial".
2. **Hoy** (inicio): "Para hacer hoy" (lista dinámica de pendientes con CTA), estado de los 3 turnos, insumos del día, meta del mes (tarjeta oscura), bitácora de hoy.
3. **Insumos**: revisión de lo leído del PDF (rol→turno recordado, cambios vs versión anterior, tabla por faena) + **avisos de lectura** que se resuelven con un clic (código inexistente en maestro, persona duplicada en dos faenas, nombre distinto). "Confirmar nombrada" sólo habilitado sin avisos abiertos. Subir nueva versión invalida la confirmación.
4. **3° turno** (cambia cada día, lo confirma Prevención antes de 20:00): estados *No hay* / *Sí · faenas habituales* / *Sí · otras labores*. Lista de la rotativa agrupada; selección múltiple o por grupo; barra sticky oscura para asignar **labor + ubicación** en bloque; "No viene"; "Copiar de la última noche" (resalta en ámbar los nuevos). No se confirma con personas sin asignar. Al confirmar se habilita el sorteo del 3° turno.
5. **Nuevo sorteo** (asistente 4 pasos): Turno → Universo (sólo se excluyen **faenas completas**, nunca personas; exclusiones automáticas por test reciente < X días y licencias; faenas chicas agrupadas) → Muestra (% o cantidad; reparto proporcional por faena con método de restos mayores; probabilidad "1 de cada N") → Sorteo (resultado agrupado por ubicación, "Testeado"/"No está" → reemplazo automático de la misma faena, correo automático, acta para firma descargable).
6. **Vista terreno (móvil)**: lista por ubicación, sólo códigos y cargos (privacidad), botones ≥52px, reemplazo en sitio, modo sin conexión (si se aprueba).
7. **Seguimiento**: período (mes/trimestre/todo), 6 KPIs, pestañas Resumen (cobertura por turno/área, testeos por mes, alertas), Historial de sorteos (bitácora, descargar acta, **verificar resultado**), Por trabajador (búsqueda por código, restringido, se registra la consulta), Descargables (PDF seguimiento, CSV registro, CSV bitácora, ZIP actas, PDF auditoría) + "Informes generados".
8. **Configuración inicial**: carga de la política, metas y reglas.
9. **Por aprobar** (funciones bloqueadas 🔒 con flujo propio y registro de decisión Aprobar/Pedir cambios/Rechazar + historial): protocolo de resultado no negativo (6 etapas), negativa a testearse (con testigo), testeos no aleatorios (post-accidente, sospecha fundada, cargo crítico, reintegro), difusión de la política, roles y permisos, resguardo de datos sensibles (Ley 19.628), contratistas, app sin conexión, aviso a control de acceso.

### Reglas críticas del sorteo (implementar en backend)
- **Aleatoriedad reproducible**: cada sorteo genera un **código de verificación** (seed). Con los mismos insumos + seed → mismo resultado. En el prototipo: PRNG `mulberry32` + hash del seed; Fisher–Yates por faena sobre la lista ordenada por código. En producción usar CSPRNG para generar el seed y PRNG determinístico para la selección; guardar seed, versión de insumos y universo.
- **Reemplazo**: semilla derivada (`seed + '-R' + n`), misma faena, excluye ya seleccionados.
- **Exclusión** por días (prop `exclusionDias`, default 30).
- **Bitácora inmutable**: las correcciones son eventos nuevos, nada se borra.
- **Confidencialidad**: resultados sólo Prevención; jefaturas ven "testeado sí/no".


## Integraciones pendientes (definir con el cliente)
- Lectura de la nombrada diaria (PDF) y de la rotativa de terminales (Excel).
- Envío de correo con el resultado del sorteo y el acta (SMTP corporativo / Microsoft 365).
- Roles: Encargado de Prevención, Equipo de terreno, Jefaturas de área, RR.HH., Gerencia.
- Retención y cifrado de resultados (Ley 19.628).

## Datos de ejemplo
Códigos, cargos e historiales son ficticios (inspirados en la nombrada del 14-10-2026).

## Capturas (`capturas/`)
1. `01-hoy.png` — Hoy en el puerto
2. `02-insumos.png` — Revisión de la nombrada
3. `03-tercer-turno.png` — Confirmar 3° turno
4. `04-nuevo-sorteo.png` — Asistente de sorteo, paso 1
5. `05-seguimiento.png` — Seguimiento
6. `06-por-aprobar.png` — Funciones por aprobar

## Archivos
```
prototipo.dc.html            ← referencia principal
support.js                   ← runtime para abrir el .dc.html
assets/pvsa-logo.png
capturas/
referencias/                 ← wireframes y direcciones visuales
insumos-ejemplo/             ← nombrada de ejemplo
```
