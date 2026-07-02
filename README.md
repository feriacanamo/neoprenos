# Cannarias Masters Cup — Campañas de email

Mailings profesionales para promocionar la **Cannarias Masters Cup 2026**
(1ª edición · Fuerteventura · 28 de noviembre de 2026), con diseño premium
canario (verde + oro) y pensados para **llegar a bandeja de entrada** gastando
lo mínimo.

## Qué hay aquí

```
emails/
  patrocinadores.html   → email para captar PATROCINADORES (B2B)
  patrocinadores.txt    → versión texto plano (multipart) de ese email
  participantes.html    → email para captar PARTICIPANTES (clubes, grow shops, pros)
  participantes.txt     → versión texto plano (multipart) de ese email
ESTRATEGIA-ENTREGABILIDAD.md  → cómo NO caer en spam y hacerlo barato (LÉELO)
README.md             → este archivo
```

> Los HTML están hechos con tablas y estilos en línea (estándar email): se ven
> bien en Gmail, Outlook, Apple Mail y móvil. Casi todo es texto + CSS y **una
> sola imagen** (el logo): así pesan poco, son baratos de enviar y entregan mejor.

## Paso 1 — Sustituye los marcadores (placeholders)

Busca y reemplaza estos textos en los 4 archivos antes de enviar:

| Marcador | Sustitúyelo por |
|---|---|
| `LOGO_URL` | URL pública del logo (ver Paso 2) |
| `INSCRIPCION_URL` | Enlace al formulario/página de inscripción |
| `BASES_URL` | Enlace para descargar las bases |
| `DOSSIER_URL` | Enlace al dossier de patrocinio |
| `CORREO_CONTACTO` | Tu email de contacto real |
| `TELEFONO` | Tu teléfono/WhatsApp (sin el +34, p. ej. `600112233`) |
| `NOMBRE_EMPRESA` | Razón social / organizador |
| `DIRECCION_POSTAL_COMPLETA` | Dirección física (obligatoria por ley) |
| `PREFERENCIAS_URL` | Página de preferencias (opcional; si no, quita ese enlace) |
| `{NOMBRE}` | Campo de personalización de AcyMailing (ver Paso 3) |
| `{unsubscribe}` `{webversion}` | Etiquetas de AcyMailing (déjalas; él las rellena) |

## Paso 2 — Sube el logo a una URL pública

El email referencia el logo por internet (`LOGO_URL`). El logo está en tu Drive:
**CANNARIAS MASTERS CUP / cannarias-site-icon.png**.

1. Súbelo a tu web/hosting (p. ej. `https://tudominio.com/img/logo-cmc.png`).
2. Pega esa URL en lugar de `LOGO_URL` en los 4 sitios donde aparece.
3. Recomendado: imagen cuadrada, ~240×240 px, PNG optimizado (<50 KB).

> Evita "incrustar" la imagen en base64 o como adjunto: muchos clientes la
> bloquean y aumenta el riesgo de spam. Mejor alojada y enlazada por HTTPS.

## Paso 3 — Personalización en AcyMailing

Cambia `{NOMBRE}` por la etiqueta real de AcyMailing, normalmente
`{subtag:name}` (o el campo que uses para el nombre del contacto). Configura un
**valor por defecto** ("Hola, profesional" / "Hola") por si el contacto no tiene
nombre, para que nunca aparezca "Hola ,".

## Paso 4 — Asuntos y preheaders (ya optimizados anti-spam)

Asuntos sobrios, cortos y sin palabras-trampa. Elige uno o haz A/B test.

### Email PARTICIPANTES
- **Asunto A:** `Cannarias Masters Cup 2026 — inscripción abierta`
- **Asunto B:** `Presenta tus muestras: copa canaria, 28 de noviembre`
- **Asunto C:** `Tu sitio en la 1ª copa canaria (plazas limitadas)`
- **Preheader:** `Fuerteventura · jurado especializado · desde 45 € por muestra.`

### Email PATROCINADORES
- **Asunto A:** `Propuesta de patrocinio — Cannarias Masters Cup 2026`
- **Asunto B:** `Tu marca en la copa canaria del sector (1ª edición)`
- **Asunto C:** `Patrocinio Cannarias Masters Cup: dossier disponible`
- **Preheader:** `Visibilidad ante clubes, grow shops y profesionales. Plazas limitadas.`

> Evita en el asunto: "marihuana/THC/weed", "gratis", "promoción", MAYÚSCULAS,
> "!!!", $$$ y exceso de emojis. Ver detalles en `ESTRATEGIA-ENTREGABILIDAD.md`.

## Paso 5 — Enviar bien (resumen)

El **gran problema de spam** no se arregla con el diseño, sino con la
configuración de envío. Lee `ESTRATEGIA-ENTREGABILIDAD.md`, pero en corto:

1. **Autentica el dominio**: SPF + DKIM + DMARC (gratis, es lo más importante).
2. **Subdominio dedicado** para enviar (no tu Gmail, no tu dominio principal).
3. **SMTP correcto**: conecta AcyMailing a **Amazon SES** (~0,10 €/1.000 emails).
   **No** uses Mailchimp/Brevo/SendGrid: prohíben cannabis y te suspenden.
4. **Warm-up**: empieza con 30–50/día y sube poco a poco.
5. **Lista opt-in y limpia**, segmentada (patrocinadores ≠ participantes).
6. **Test en mail-tester.com** antes de cada campaña (apunta a 9–10/10).

## Notas de diseño

- Paleta tomada del logo: verde profundo `#0e2a1a` / `#0a1f13`, oro `#caa83c`,
  marfil `#f4eccf`, verde texto `#d8e2d2`.
- Ancho 600 px, responsive (se apila en móvil).
- Tipografías "web-safe" (Georgia + Trebuchet) para que se vean igual en todos
  los clientes sin cargar fuentes externas.
- Datos editables (categorías, precios, niveles de patrocinio): ajústalos cuando
  cierres las bases definitivas. Los **niveles de patrocinio** (Platino/Oro/Plata)
  son una propuesta de partida; defínelos con tus tarifas reales.

---
**Importante (legal/operativo):** según tus propios documentos, el proyecto está
*pendiente de revisión legal, financiera y operativa*. Antes de enviar de forma
masiva, valida el texto con esa revisión (especialmente todo lo relativo a
muestras, dispensación y participación). El pie ya incluye el aviso de evento
privado, profesional y para mayores de edad.
