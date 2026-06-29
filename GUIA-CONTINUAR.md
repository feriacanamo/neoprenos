# Guía para continuar — Cannarias Masters Cup (campañas de email)

Documento de control para retomar el proyecto desde tu ordenador. Resume **qué
está hecho**, **dónde está cada cosa** y **qué falta**.

_Última actualización: 29 de junio de 2026._

---

## 1. Estado actual

✅ **Hecho:**
- 2 emails de captación (HTML + texto plano): **patrocinadores** y **participantes**.
- Diseño premium canario (verde + oro) sacado de tu logo, responsive y ligero.
- Documento de estrategia anti-spam y de coste mínimo.
- README con instrucciones, asuntos optimizados y placeholders.

🟡 **Pendiente (lo haces tú):**
- Rellenar los placeholders (URLs, contacto, dirección postal). Ver §4.
- Subir el logo a una URL pública y ponerla en los emails.
- Configurar el envío (autenticación DNS + Amazon SES en AcyMailing). Ver §5.
- Validar legalmente el texto antes del envío masivo.
- Cerrar los **niveles de patrocinio** con tarifas reales (ahora son propuesta).

---

## 2. Dónde está todo

**En tu Google Drive** (carpeta nueva):
`CANNARIAS MASTERS CUP / Campañas Email (Mailing)`
→ https://drive.google.com/drive/folders/1yymc7UtYKHMeDgqqdYrd-GxEKzGeSQpv

**En el repositorio Git** (`feriacanamo/neoprenos`, rama
`claude/canarias-masters-cup-emails-cxay71`):
```
emails/patrocinadores.html
emails/patrocinadores.txt
emails/participantes.html
emails/participantes.txt
ESTRATEGIA-ENTREGABILIDAD.md
README.md
GUIA-CONTINUAR.md   ← este archivo
```

**En tu ordenador:** descarga la carpeta de Drive a
`C:\Users\lsdio\OneDrive\Documentos\Cannabis master cup` (clic derecho →
Descargar, o sincroniza OneDrive/Drive).

> Los `.html` ábrelos con doble clic para previsualizarlos en el navegador.
> Edítalos con Bloc de notas, VS Code o Notepad++ (no con Word).

---

## 3. Datos del evento (fuente única de la verdad)

| Dato | Valor |
|---|---|
| Nombre | Cannarias Masters Cup 2026 (1ª edición) |
| Fecha | 28 de noviembre de 2026 |
| Sede | Fuerteventura (villa privada) |
| Formato | Evento privado y premium: catering, charlas, música, networking, premios |
| Categorías (propuesta 1ª ed.) | Sativa · Índica · Extracciones (Hash/Rosin) |
| Cuota participación | 50 € una muestra · 40 €/muestra desde dos |
| Jurado | 3 jueces especializados por categoría (anónimos + estrella invitada) |
| Se valora | Aroma, sabor y presencia |
| Quién participa | Clubes/asociaciones, grow shops, bancos de semillas, empresas del sector y profesionales (mayores de edad) |
| Cierre inscripciones | Mediados/finales de octubre |
| Paleta | Verde `#0e2a1a` / `#0a1f13` · Oro `#caa83c` · Marfil `#f4eccf` |

---

## 4. Placeholders a sustituir (en los 4 archivos de email)

| Marcador | Sustitúyelo por |
|---|---|
| `LOGO_URL` | URL pública del logo |
| `INSCRIPCION_URL` | Página/formulario de inscripción |
| `BASES_URL` | Descarga de las bases |
| `DOSSIER_URL` | Dossier de patrocinio |
| `CORREO_CONTACTO` | Email de contacto real |
| `TELEFONO` | Teléfono/WhatsApp (sin +34) |
| `NOMBRE_EMPRESA` | Razón social / organizador |
| `DIRECCION_POSTAL_COMPLETA` | Dirección física (obligatoria por ley) |
| `PREFERENCIAS_URL` | Página de preferencias (opcional) |
| `{NOMBRE}` | Etiqueta de AcyMailing (p. ej. `{subtag:name}`) |
| `{unsubscribe}` `{webversion}` | Etiquetas de AcyMailing (dejarlas tal cual) |

> Truco: en VS Code, Ctrl+H (buscar y reemplazar) en cada archivo.

---

## 5. Próximos pasos en orden (entregabilidad)

**Vía de envío elegida: AcyMailing Sending Service** (el que ya está pagado).
Guía detallada paso a paso en **`CONFIGURACION-ACYMAILING.md`**. En corto:

1. **Soporte AcyMailing**: confirma por escrito que permiten temática cannabis (Paso 0).
2. **Activa** el Sending Service en AcyMailing (Configuración → Sending method).
3. **Publica las entradas DNS** que te muestra AcyMailing: SPF + DKIM (+ Return-Path) + DMARC.
4. **Verifica** en AcyMailing que está en verde (y comprueba en Gmail → "Mostrar original").
5. **Warm-up**: empieza con 30–50 envíos/día y sube gradualmente.
6. **Lista** opt-in, limpia y segmentada (patrocinadores ≠ participantes).
7. **Test** en mail-tester.com (≥9/10) y prueba en Gmail/Outlook/móvil.

> Plan B si te restringen por temática: **Amazon SES** (mismas plantillas, solo
> cambia el método de envío). Detalle en `ESTRATEGIA-ENTREGABILIDAD.md`.

---

## 6. Ideas para ampliar (cuando quieras)

- 3er email de **recordatorio/seguimiento** para quien no abra el primero.
- Versión en **inglés** (sector internacional / turismo canario).
- **Landing** de inscripción y de patrocinio a juego con el diseño.
- **Dossier de patrocinio** en PDF con tarifas (hoy enlazado como `DOSSIER_URL`).
- Email de **confirmación** automático al inscribirse.

---

## 7. Aviso legal/operativo

Según tus propios documentos, el proyecto está **pendiente de revisión legal,
financiera y operativa**. Antes de cualquier envío masivo, valida el texto
(sobre todo lo relativo a muestras, dispensación y participación). El pie de los
emails ya incluye el aviso de evento privado, profesional y para mayores de edad.
