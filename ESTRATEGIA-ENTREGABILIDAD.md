# Estrategia de entregabilidad y coste — Cannarias Masters Cup

> Objetivo: que los emails lleguen a **bandeja de entrada** (no a spam) gastando lo **mínimo** posible.
> Tu caso tiene dos dificultades sumadas: **contenido de cannabis** (los filtros lo penalizan) y **AcyMailing enviando desde tu propio servidor** (reputación de IP baja y, muchas veces, sin autenticación). Esto se arregla. Abajo está el plan en orden de impacto.

---

## TL;DR — qué hacer primero (de mayor a menor impacto)

1. **Autentica tu dominio: SPF + DKIM + DMARC.** Es gratis y es el 80% del problema. Sin esto, Gmail/Outlook te mandan a spam casi seguro.
2. **Envía desde un subdominio dedicado** (p. ej. `info.tudominio.com` o `cmc.tudominio.com`), no desde el dominio principal. Aísla la reputación.
3. **No uses Mailchimp/Brevo/SendGrid para contenido de cannabis**: te suspenden la cuenta (lo prohíben sus términos). Usa **Amazon SES** o un **SMTP cannabis-friendly**, conectado a tu AcyMailing.
4. **Calienta la reputación**: empieza enviando poco (30–50/día) y sube gradualmente. No mandes 2.000 de golpe el primer día.
5. **Lista 100% opt-in y limpia.** Nada de listas compradas. Una sola queja de spam por correo basura hace más daño que 100 envíos buenos.
6. **Cuida las palabras** del asunto y el preheader (evita "marihuana", "THC", "gratis", "promoción", signos $$$, MAYÚSCULAS, exceso de emojis).
7. **Email ligero**: mucho texto real, poca imagen (las plantillas ya están así). Siempre versión texto + HTML.

---

## 1. Autenticación del dominio (GRATIS — lo más importante)

Estos tres registros DNS le dicen a Gmail/Outlook que tus correos son legítimos. Se configuran **una vez** en el panel DNS de tu dominio.

### SPF
Autoriza qué servidores pueden enviar en tu nombre. Un único registro TXT en el dominio de envío:
```
Tipo: TXT
Host: @  (o el subdominio: cmc)
Valor: v=spf1 include:amazonses.com ~all
```
> El `include:` depende del proveedor que uses para enviar (SES, tu hosting, etc.). Cada proveedor te da el suyo. **No tengas dos registros SPF**: se combinan en uno solo.

### DKIM
Firma criptográfica de cada correo. El proveedor de envío (SES, tu hosting con AcyMailing, etc.) te da 1–3 registros CNAME/TXT que pegas en el DNS. Imprescindible.

### DMARC
Le dice al receptor qué hacer si SPF/DKIM fallan, y te manda informes. Empieza en modo "solo observar":
```
Tipo: TXT
Host: _dmarc  (o _dmarc.cmc)
Valor: v=DMARC1; p=none; rua=mailto:dmarc@tudominio.com; fo=1
```
> Empieza con `p=none` durante 2–4 semanas para ver informes sin bloquear nada. Cuando todo esté verde, sube a `p=quarantine` y luego `p=reject`.

### Cómo comprobar que está bien (gratis)
- Envíate un correo a una cuenta de **Gmail** → abre el mensaje → "Mostrar original" → deben aparecer **PASS** en SPF, DKIM y DMARC.
- Herramientas: **mail-tester.com** (puntúa tu correo sobre 10; apunta a 9–10), **mxtoolbox.com**, **dmarcian** o **Google Postmaster Tools** (registra ahí tu dominio: te da reputación y % de spam reales de Gmail).

---

## 2. Subdominio de envío dedicado

Envía las campañas desde un subdominio (p. ej. `noreply@cmc.tudominio.com` o `eventos@info.tudominio.com`), **no** desde `@tudominio.com` ni desde un Gmail/Hotmail gratuito.

Por qué:
- Si una campaña va mal, **no quemas** la reputación del dominio principal (donde está tu correo de trabajo).
- Permite políticas DMARC distintas para marketing y para correo personal.

> **Nunca** pongas en el "From" un `@gmail.com` o `@hotmail.com`: DMARC de esos dominios hará que te marquen como suplantación y caes directo a spam.

---

## 3. Cómo enviar barato SIN que te baneen por cannabis

Este es el punto delicado. La mayoría de plataformas de email marketing **prohíben expresamente el contenido de cannabis** en sus términos y suspenden la cuenta (a veces reteniendo tu lista): **Mailchimp, Brevo/Sendinblue, SendGrid, Mailjet, Constant Contact, ActiveCampaign**. Evítalas para estas campañas.

Tienes AcyMailing, que es solo el **software** que arma y dispara el correo. El problema es el **canal de salida (SMTP)**. Opciones, de más barata a más cómoda:

| Opción | Coste aprox. | Cannabis | Esfuerzo | Recomendado para |
|---|---|---|---|---|
| **A. AcyMailing + Amazon SES** | ~0,10 €/1.000 emails | Tolerante (relé técnico, sin prohibición temática explícita; cumple su política antispam y opt-in) | Medio (configurar SES + DNS) | **La mejor relación coste/entregabilidad.** Tu opción por defecto. |
| **B. AcyMailing + SMTP de tu hosting** | Incluido en tu hosting | Depende del hosting | Bajo | Volumen pequeño (<200/día) y solo si el hosting tiene buena IP + autenticas DNS |
| **C. SMTP cannabis-friendly especializado** | Variable (de pago) | Sí, explícito | Bajo | Si no quieres pelearte con SES y prefieres soporte que "entiende" el sector |
| **D. Mailchimp/Brevo/etc.** | "Gratis"/barato | **NO — te suspenden** | Bajo | ❌ No usar |

### Recomendación: Opción A (AcyMailing + Amazon SES)
- **Coste real**: 0,10 $ por cada 1.000 correos. Para una base de, p. ej., 3.000 contactos × 4 campañas = 12.000 envíos = **~1,2 $**. Prácticamente gratis.
- SES es un relé de infraestructura: no prohíbe la temática como tal, pero **sí exige** opt-in real, gestión de bajas y tasas de queja/rebote bajas. Cumpliendo eso, va perfecto.
- Pasos: crear cuenta AWS → SES → verificar tu dominio/subdominio (te da los DKIM) → salir del "sandbox" (pides acceso a producción explicando que es marketing con lista propia opt-in) → coger las credenciales SMTP → pegarlas en AcyMailing (Configuración → Servidor de envío → SMTP).
- Configura en SES un **SNS para rebotes y quejas** y conéctalo a AcyMailing para que limpie solo la lista.

> Si AWS te resulta complejo, la Opción C (un SMTP cannabis-friendly de pago) te ahorra el lío técnico a cambio de unos euros al mes. Pero técnicamente SES es lo más barato y robusto.

---

## 4. Calentamiento ("warm-up") de la reputación

Una IP/dominio nuevos no tienen historial. Si el primer día disparas miles, los filtros lo leen como spam. Sube el volumen poco a poco:

| Día | Envíos/día |
|---|---|
| 1–2 | 30–50 (a tus contactos más cercanos, que seguro abren) |
| 3–4 | 100 |
| 5–7 | 250 |
| 2ª semana | 500–1.000 |
| 3ª semana | Volumen completo |

Empieza por los contactos que **más te van a abrir** (clientes, conocidos del sector): las aperturas tempranas construyen reputación positiva.

---

## 5. Lista: opt-in, limpieza y segmentación

- **Solo contactos que consintieron** recibir tu comunicación. Listas compradas o scrapeadas = quejas, rebotes y muerte de la reputación.
- **Limpia antes de enviar**: quita emails inválidos, duplicados y rebotes anteriores. Un rebote alto (>3–5%) te marca como spammer. Herramientas de validación de lista existen (algunas con free tier).
- **Segmenta**: la campaña de **patrocinadores** va a marcas/empresas; la de **participantes** va a clubes/grow shops/profesionales. No mezcles: relevancia = más aperturas = mejor reputación.
- **Re-permiso**: si tu lista es antigua o dudas del consentimiento, manda primero un correo corto de "¿quieres seguir recibiendo novedades de la copa?" y quédate solo con quien responda/haga clic.
- **Gestiona la baja de verdad**: el enlace `{unsubscribe}` (AcyMailing lo inserta) debe funcionar y respetarse al instante. Es obligatorio por RGPD y baja muchísimo las quejas.

---

## 6. Contenido: qué dispara los filtros (y qué hacer)

### En el asunto y el preheader (lo que más pesa)
- **Evita** en el asunto: "marihuana", "cannabis", "THC", "weed", "porro", "gratis", "promoción", "oferta", "$$$", "100%", "urgente", signos de exclamación múltiples (!!!), TODO EN MAYÚSCULAS, exceso de emojis.
- **Sí funciona**: tono sobrio y profesional, nombre del evento, beneficio claro, una pizca de urgencia real (plazas limitadas).
- Mantén el asunto **corto** (35–50 caracteres) y que **case** con el contenido (nada de clickbait).

### En el cuerpo
- **Ratio texto/imagen alto**: las plantillas ya están hechas casi todo con texto y CSS, con **una sola imagen** (el logo). No metas el email como una imagen gigante: es la receta del spam.
- **Siempre multipart**: HTML + texto plano (por eso tienes los `.txt`). AcyMailing genera la versión texto; revisa que no esté vacía.
- Pon **enlaces de tu propio dominio**, no acortadores (bit.ly y similares huelen a spam).
- **Pocos enlaces** y a destinos limpios. Evita adjuntar PDF pesados: enlaza al dossier/bases en tu web o Drive.
- Incluye **dirección postal física** y enlace de baja (ya están en el pie). Es requisito legal y señal de legitimidad.
- Revisa que el **From name** sea reconocible ("Cannarias Masters Cup") y el **reply-to** sea un buzón real que atiendas.

### Pasa el test antes de cada campaña
Envía la campaña a **mail-tester.com** y a una cuenta tuya de Gmail, Outlook y (si puedes) un correo de hosting. Objetivo: **9/10 o más** en mail-tester y bandeja de entrada en Gmail. Si algo falla, te dice exactamente qué.

---

## 7. Configuración concreta en AcyMailing

1. **Servidor de envío**: Configuración → cambia de "PHP mail" a **SMTP** con las credenciales de SES (Opción A). PHP mail desde hosting compartido es de lo que más spam genera.
2. **Sender**: nombre "Cannarias Masters Cup", from `eventos@cmc.tudominio.com` (subdominio autenticado), reply-to un buzón real.
3. **Bounce handling**: activa el procesamiento de rebotes para que limpie la lista automáticamente.
4. **Velocidad de envío**: limita los emails por hora (p. ej. 100–200/hora) durante el warm-up. AcyMailing lo permite en la cola de envío.
5. **Tracking**: activa aperturas/clics, pero usa un **dominio de tracking propio** si AcyMailing lo permite (mejor que un dominio genérico compartido).
6. **Versión texto**: comprueba que cada campaña lleva su parte de texto plano (pega los `.txt` si hace falta).
7. **Test**: usa la función de envío de prueba antes de cada disparo real.

---

## 8. Coste total estimado

| Concepto | Coste |
|---|---|
| AcyMailing | Ya lo tienes (0 €) |
| SPF / DKIM / DMARC | 0 € (config DNS) |
| Amazon SES | ~0,10 €/1.000 emails (céntimos por campaña) |
| Subdominio | 0 € (ya incluido en tu dominio) |
| mail-tester / Postmaster Tools | 0 € |
| **Total realista** | **Prácticamente 0 € + tu tiempo de configuración** |

El gasto no es el problema; el problema era la **configuración**. Con SES + autenticación DNS resuelves entregabilidad y coste a la vez.

---

## 9. Checklist antes de pulsar "Enviar"

- [ ] SPF, DKIM y DMARC en verde (comprobado en Gmail → "Mostrar original")
- [ ] Envío desde subdominio dedicado y autenticado
- [ ] SMTP = Amazon SES (o cannabis-friendly), NO Mailchimp/Brevo
- [ ] Lista limpia, opt-in y segmentada (patrocinadores ≠ participantes)
- [ ] Warm-up respetado (no disparar todo el primer día)
- [ ] Asunto sobrio, corto, sin palabras-trampa
- [ ] Email con texto + HTML (multipart), una sola imagen
- [ ] Enlace de baja `{unsubscribe}` funcional + dirección postal
- [ ] LOGO_URL, INSCRIPCION_URL, DOSSIER_URL, etc. sustituidos por URLs reales
- [ ] Puntuación ≥ 9/10 en mail-tester.com
- [ ] Probado en Gmail + Outlook + móvil
```
