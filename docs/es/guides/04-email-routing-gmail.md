# Guía 04 — Cloudflare Email Routing y Gmail

[Inicio](../../../README.es.md) | [Arquitectura](../architecture.md) | [English](../../en/guides/04-email-routing-gmail.md)

## 1. Objetivo

Configurar la recepción de mensajes dirigidos a una dirección de dominio propio mediante Cloudflare Email Routing, con reenvío hacia una cuenta Gmail existente.

La arquitectura de referencia es:

Remitente externo → Cloudflare Email Routing → Gmail.

Esta solución proporciona reenvío de correo, no un buzón empresarial independiente ni un servicio de envío SMTP.

## 2. Requisitos

- Dominio con DNS administrados en Cloudflare.
- Acceso administrativo a Cloudflare.
- Cuenta Gmail activa que recibirá los mensajes.
- Conocimiento de los registros MX existentes.
- Autenticación multifactor habilitada cuando sea posible.

Antes de continuar, comprobar si el dominio ya utiliza un proveedor de correo. Cambiar sus MX puede interrumpir la recepción existente.

## 3. Evaluación previa

Determinar:

- Qué direcciones profesionales se necesitan.
- Quién administrará la recepción.
- Qué buzón almacenará los mensajes.
- Si existen obligaciones de privacidad y conservación.
- Qué alternativa se utilizará si el reenvío deja de funcionar.

Ejemplo de esta guía:

- Dirección pública: `contact@example.com`.
- Destino: `recipient@gmail.com`.

Son direcciones ilustrativas, no cuentas reales.

## 4. Activar Email Routing

En el panel de Cloudflare:

1. Seleccionar la cuenta correspondiente.
2. Abrir Compute > Email Service > Email Routing.
3. Elegir el dominio.
4. Iniciar el proceso de incorporación del dominio, si todavía no está configurado.
5. Revisar los registros DNS que Cloudflare propone administrar.
6. Completar la activación.

Los nombres de menú pueden variar si Cloudflare modifica su interfaz.

No sustituir los MX de un proveedor existente sin preparar previamente la migración.

## 5. Verificar la dirección de destino

1. Abrir Destination Addresses.
2. Añadir `recipient@gmail.com`.
3. Abrir Gmail.
4. Localizar el mensaje de verificación de Cloudflare.
5. Confirmar la dirección.
6. Comprobar que figura como verificada.

Una regla que depende de un destino sin verificar no puede comenzar a reenviar normalmente.

## 6. Crear la regla de recepción

En Routing Rules:

1. Seleccionar Create routing rule.
2. Introducir el nombre local `contact`.
3. Seleccionar `example.com`.
4. Elegir la acción de enviar a una dirección.
5. Seleccionar `recipient@gmail.com`.
6. Guardar la regla.
7. Confirmar que aparece activa.

La regla representa:

`contact@example.com` → `recipient@gmail.com`.

No es necesario habilitar catch-all para recibir únicamente las direcciones configuradas.

## 7. Registros DNS

Cloudflare Email Routing utiliza registros MX para recibir mensajes del dominio.

Ejemplos de destinos utilizados por el servicio:

- `route1.mx.cloudflare.net`
- `route2.mx.cloudflare.net`
- `route3.mx.cloudflare.net`

Las prioridades y los registros definitivos deben obtenerse de Cloudflare, no copiarse de otro dominio.

Cloudflare también puede requerir registros TXT de SPF y DKIM para su funcionamiento.

No crear varios registros SPF independientes para el mismo nombre: si existen varios servicios autorizados, revisar cómo combinarlos correctamente.

El correo saliente y la autenticación completa del dominio se tratarán en las Guías 05 y 06.

## 8. Comprobar DNS

En Windows PowerShell:

```powershell
Resolve-DnsName example.com -Type MX
Resolve-DnsName example.com -Type TXT
```

Reemplazar `example.com` por el dominio correspondiente.

Comprobar que los MX son coherentes con el servicio de recepción elegido.

La resolución DNS correcta no garantiza por sí sola la entrega de mensajes.

## 9. Primera prueba de recepción

Desde una cuenta externa diferente de Gmail:

1. Enviar un mensaje a `contact@example.com`.
2. Utilizar un asunto único.
3. Comprobar la recepción en `recipient@gmail.com`.
4. Revisar remitente, destinatario original, fecha y contenido.
5. Revisar spam si no aparece en la bandeja principal.
6. Registrar el resultado de la prueba.

Realizar una segunda prueba desde otro proveedor cuando sea posible.

Las pruebas realizadas en JH-Web incluyeron mensajes enviados desde una cuenta externa y recepción en Gmail.

## 10. Diagnóstico

| Síntoma | Comprobación |
|---|---|
| No llega el mensaje | Revisar MX, regla y destino |
| Destino pendiente | Completar verificación |
| Regla desactivada | Revisar estado en Routing Rules |
| Dirección rechazada | Confirmar la coincidencia exacta de la regla |
| Mensaje en spam | Revisar filtros del buzón de destino |
| Recepción antigua interrumpida | Comprobar migración de MX |
| Reenvío fallido | Revisar actividad y errores del servicio |

En una migración, los servidores externos pueden utilizar información DNS almacenada en caché. Evitar dar por finalizado el cambio basándose en una sola prueba.

## 11. Catch-all

Una regla catch-all permite tratar mensajes enviados a direcciones no configuradas individualmente.

Puede ser útil en algunos escenarios, pero también puede aumentar la recepción de mensajes no deseados o enviados por error.

En la implementación de referencia de JH-Web se mantuvo desactivada.

## 12. Límites y consideraciones

- Cloudflare Email Routing no proporciona un buzón IMAP independiente.
- No permite enviar correo profesional por sí mismo.
- Gmail almacena los mensajes reenviados.
- Las políticas antispam y autenticación de remitentes pueden afectar al reenvío.
- No sustituye automáticamente los controles de un servicio empresarial administrado.
- Se deben revisar las condiciones, límites y obligaciones de protección de datos.

Para procesos críticos, información sensible o necesidad de soporte contractual, evaluar un servicio empresarial de correo completo.

## 13. Resultado esperado

Una dirección profesional recibe mensajes enviados desde Internet y los entrega en una cuenta Gmail verificada.

Todavía no se ha configurado el envío desde dicha dirección.

La Guía 05 describirá el cliente Thunderbird y las opciones de salida SMTP, incluyendo restricciones de proveedor.

## 14. Referencias oficiales

- [Cloudflare — Route emails](https://developers.cloudflare.com/email-service/get-started/route-emails/)
- [Cloudflare — Routing rules and addresses](https://developers.cloudflare.com/email-service/configuration/email-routing-addresses/)
- [Cloudflare — Domain configuration](https://developers.cloudflare.com/email-service/configuration/domains/)
- [Cloudflare — Email troubleshooting](https://developers.cloudflare.com/email-service/reference/troubleshooting/)
- [Implementación JH-Web](https://juanherbozo.com/es/projects/jh-web-personal-digital-platform/)