# Configuración del AcyMailing Sending Service — Cannarias Masters Cup

Guía paso a paso para enviar las campañas con el **servicio de envío propio de
AcyMailing** (el que ya tienes pagado), con buena entregabilidad y sin caer en spam.

> Los **valores exactos** de SPF/DKIM/DMARC y del Return-Path son **únicos de tu
> cuenta**: aparecen dentro de AcyMailing cuando activas el servicio. Esta guía te
> dice dónde verlos y qué hacer con cada uno; no los inventes ni los copies de otra web.

---

## Paso 0 — Confirma la política de contenido (hazlo, pero seguimos)

Eres cliente de pago: abre un ticket a soporte de AcyMailing y pregunta **por escrito**:

> "¿Permite vuestra Acceptable Use Policy enviar campañas B2B de un evento
> profesional del sector cannábico (una copa cannábica) a través del Sending
> Service? ¿Hay restricción de temática cannabis/CBD?"

- Si dicen **sí** → adelante con esta guía.
- Si dicen **no/evasivo** → no arriesgues la cuenta; pásate a Amazon SES
  (ver `ESTRATEGIA-ENTREGABILIDAD.md`, Opción A) y usa AcyMailing solo como editor.

Guarda su respuesta. Mientras tanto puedes ir dejando la configuración lista.

---

## Paso 1 — Activar el Sending Service en AcyMailing

1. AcyMailing → **Configuración** (Configuration).
2. Pestaña **"Sending method"** / **"AcyMailing Sending Service"**.
3. Activa el servicio (requiere suscripción/créditos activos: comprueba en
   **"Managing the license"** cuántos **créditos** te quedan y cuándo se renuevan).
4. Al activarlo, AcyMailing te mostrará las **entradas DNS** que debes publicar
   (SPF, DKIM, normalmente un Return-Path/CNAME para alineación de rebotes, y la
   recomendación de DMARC). **Déjalo abierto**: lo necesitas en el Paso 3.

---

## Paso 2 — Decide el remitente (From) y subdominio

- **From name:** `Cannarias Masters Cup`
- **From email:** un buzón de **tu dominio autenticado**. Recomendado un
  **subdominio de envío**, p. ej. `eventos@cmc.tudominio.com`, para no mezclar la
  reputación con tu correo personal.
- **Reply-to:** un buzón **real que atiendas** (para responder a interesados).
- ⚠️ Nunca pongas un `@gmail.com` / `@hotmail.com` como From: rompe DMARC y vas a spam.

> Si no quieres crear subdominio, puedes usar `@tudominio.com`, pero el subdominio
> dedicado es más seguro para campañas.

---

## Paso 3 — Publicar las entradas DNS (lo más importante)

En el panel DNS de tu **dominio** (en tu registrador o en el hosting: cPanel/Plesk →
"Zona DNS"), añade **exactamente** los registros que te muestra AcyMailing:

1. **SPF** (registro TXT en el dominio/subdominio de envío):
   - Si ya tienes un SPF, **no crees otro**: añade el `include:` de AcyMailing
     dentro del que existe. Solo puede haber **un** registro SPF.
   - Ejemplo de forma (usa el valor real que te dé AcyMailing):
     `v=spf1 include:<dominio_de_acymailing> ~all`
2. **DKIM** (registro TXT o CNAME que te da AcyMailing, con su selector). Cópialo tal cual.
3. **Return-Path / bounce** (si AcyMailing pide un CNAME para el Return-Path,
   añádelo: mejora la alineación y el manejo de rebotes).
4. **DMARC** (TXT en `_dmarc.tudominio.com`), empieza suave:
   `v=DMARC1; p=none; rua=mailto:dmarc@tudominio.com; fo=1`
   - A las 2–4 semanas, si todo va verde, sube a `p=quarantine` y luego `p=reject`.

> La propagación tarda de minutos a 48 h. Paciencia antes de validar.

---

## Paso 4 — Verificar en AcyMailing

1. Vuelve a la configuración del Sending Service.
2. Pulsa **"Verificar"/"Check"** (o equivalente): debe poner en **verde**
   SPF y DKIM (y Return-Path si aplica).
3. Si sigue en rojo: espera más a la propagación o revisa que copiaste el valor
   completo (sin espacios, sin duplicar el SPF).

Comprobación cruzada gratis:
- Envíate un correo de prueba a **Gmail** → abrir → **"Mostrar original"** →
  deben aparecer **PASS** en SPF, DKIM y DMARC.
- O usa **mail-tester.com** (objetivo: **9–10/10**).

---

## Paso 5 — Warm-up (calentar la reputación)

Aunque las IPs de AcyMailing están gestionadas, **tu dominio** es nuevo enviando.
Sube el volumen poco a poco (en AcyMailing puedes limitar emails/hora en la cola):

| Día | Envíos/día |
|---|---|
| 1–2 | 30–50 (a tus contactos más cercanos, que abrirán seguro) |
| 3–4 | 100 |
| 5–7 | 250 |
| 2ª semana | 500–1.000 |
| 3ª semana | Volumen completo |

Empieza por quien más te vaya a abrir: las aperturas tempranas construyen reputación.

---

## Paso 6 — Antes de cada campaña (checklist)

- [ ] Soporte de AcyMailing confirmó que la temática está permitida (Paso 0)
- [ ] SPF, DKIM (y Return-Path) en **verde** en AcyMailing
- [ ] DMARC publicado (`p=none` al principio)
- [ ] From con subdominio autenticado + Reply-to real
- [ ] Lista **opt-in**, limpia y **segmentada** (patrocinadores ≠ participantes)
- [ ] Warm-up respetado (no disparar toda la lista el primer día)
- [ ] Placeholders sustituidos (LOGO_URL, INSCRIPCION_URL, DOSSIER_URL, dirección…)
- [ ] Email **multipart** (HTML + texto): pega los `.txt` si AcyMailing no los genera
- [ ] Asunto sobrio y corto, **sin** "marihuana/THC/gratis/promoción", sin MAYÚSCULAS ni !!!
- [ ] Enlace de baja `{unsubscribe}` funcional + dirección postal en el pie
- [ ] Prueba en **mail-tester.com** (≥9/10) y en Gmail/Outlook/móvil

---

## Notas

- Si más adelante envías mucho volumen y quieres aislar del todo la reputación,
  AcyMailing ofrece **IP dedicada** bajo petición (suele requerir licencia
  multisitio). Para una primera edición, con IP compartida + buen warm-up basta.
- Si en algún momento te suspenden por temática, el plan B sigue siendo
  **Amazon SES** (mismas plantillas, solo cambia el método de envío en AcyMailing).
- El resto de buenas prácticas (lista, contenido, asuntos) están en
  `ESTRATEGIA-ENTREGABILIDAD.md`.
