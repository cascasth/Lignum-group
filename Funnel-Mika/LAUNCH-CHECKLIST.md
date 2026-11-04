# Checklist de lanzamiento

Antes de mandar tráfico pagado a un funnel nuevo hecho con este esqueleto,
repasa esto. Son cosas aprendidas de la primera vez que se adaptó (el funnel
de Archivo de Identidad) — no son obvias hasta que fallan en producción.

## Contenido

- [ ] Definida la lógica central (pregunta → resultado) antes de escribir el
      resto del copy — todo lo demás se apoya en eso.
- [ ] Guion completo de todos los audios narrados escrito ANTES de grabar,
      para grabar todo en una sola sesión.
- [ ] Todos los `[MARCADORES_ENTRE_CORCHETES]` del código reemplazados (ver
      `TEMPLATE.md` → tabla de valores a configurar). Verifica con:
      ```
      grep -rn "\[.*\]" app --include="*.tsx" | grep -v "className\|style="
      ```

## Assets

- [ ] Todas las imágenes de fondo tipo "tarjeta" mantienen la proporción
      550×750 (o la que hayas definido) — si cambia, hay que recalcular los
      porcentajes de `padding` en el código.
- [ ] Video del VSL comprimido — el del funnel original pesaba 20MB; es
      tráfico de anuncios pagado, mucha gente lo ve con datos móviles.
- [ ] Todos los archivos de `public/ASSETS.md` presentes y con el nombre
      exacto que espera el código.

## El bug del audio — que no se repita

- [ ] Cualquier audio narrado que se dispare con `setTimeout` después de un
      clic está "desbloqueado" dentro del gesto de clic mismo (crear el
      `Audio()`, reproducir en silencio y pausar, todo de forma síncrona en
      el handler del clic — luego solo se sube el volumen y se reanuda).
      Si no, Safari/iOS y los navegadores integrados de Instagram/Facebook
      lo bloquean en silencio sin ningún error visible.
- [ ] Probado el flujo completo desde el navegador integrado de
      Instagram/Facebook (abriendo el link del anuncio de prueba real), no
      solo desde Chrome/Safari normal.

## Cosas de plataforma que pueden tumbar el anuncio, no el código

- [ ] Revisado el copy del anuncio (no solo el funnel) contra la política de
      Meta sobre "personal attributes" — anuncios que aseguran o insinúan
      atributos personales del usuario (salud, personalidad, vida amorosa,
      etc.) se rechazan. Temas de signos/numerología/tarot rozan esto fácil.
- [ ] Si el funnel simula la interfaz de WhatsApp (como el original), ten en
      cuenta que puede generar rechazo en la revisión de anuncios o
      reclamos de marca — no es solo diseño.

## Tracking

- [ ] Pixel de Meta nuevo (uno por cuenta de anuncios) configurado en
      `app/layout.tsx` y `app/meta-pixel.tsx`.
- [ ] Proyecto de Microsoft Clarity nuevo configurado en `app/layout.tsx`.
- [ ] Verificado en Meta Events Manager que `Lead` e `InitiateCheckout`
      (o los eventos que definas) llegan correctamente ANTES de gastar
      presupuesto real.

## Infraestructura

- [ ] Subdominio apuntado a Vercel (CNAME → `cname.vercel-dns.com`, agregado
      en Vercel → Settings → Domains del proyecto).
- [ ] `metadataBase` en `app/layout.tsx` actualizado al subdominio real.
- [ ] Link de pago final probado de punta a punta con un clic real, no solo
      revisado en el código.
