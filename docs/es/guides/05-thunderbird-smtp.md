# Guía 05 — Thunderbird y configuración SMTP

[Inicio](../../../README.es.md) | [Arquitectura](../architecture.md) | [English](../../en/guides/05-thunderbird-smtp.md)

## 1. Objetivo

Configurar una identidad de envío profesional en Thunderbird utilizando un servidor SMTP externo.

Arquitectura de referencia:

Thunderbird → Proveedor SMTP → Destinatario externo.

Esta guía describe las operaciones de un cliente de correo. No convierte Cloudflare Email Routing en un servicio SMTP ni proporciona automáticamente un buzón empresarial.

## 2. Requisitos

- Thunderbird instalado y actualizado.
- Una cuenta de correo configurada en Thunderbird.
- Dirección profesional bajo un dominio administrado.
- Proveedor SMTP autorizado para el uso previsto.
- Credenciales SMTP suministradas por ese proveedor.
- Configuración de autenticación del dominio cuando corresponda.

No es necesario cambiar los servidores de entrada IMAP para incorporar una identidad adicional de envío.

## 3. Separar identidad, buzón y transporte

Una identidad define el nombre y la dirección mostrados en el encabezado From.

El buzón almacena los mensajes recibidos.

El servidor SMTP transporta los mensajes salientes.

Son funciones distintas. Una dirección profesional puede recibir correo mediante reenvío y enviar mediante otro servicio, si los proveedores y sus condiciones lo permiten.

## 4. Elegir el proveedor SMTP

Antes de configurar Thunderbird, comprobar:

1. Que el proveedor autoriza el tipo de mensajes que se enviarán.
2. Que acepta la dirección o dominio remitente.
3. Que dispone de autenticación SMTP.
4. Que permite conexiones cifradas.
5. Que sus límites y políticas son adecuados.
6. Que ofrece mecanismos de recuperación y soporte suficientes.

Para correspondencia empresarial general, valorar un proveedor de correo profesional con servicio SMTP asociado.

## 5. Registrar un servidor SMTP

En Thunderbird:

1. Abrir Account Settings / Configuración de las cuentas.
2. Localizar Outgoing Server (SMTP) / Servidor de salida (SMTP).
3. Seleccionar Add / Añadir.
4. Introducir el servidor y puerto proporcionados.
5. Seleccionar el método de seguridad indicado.
6. Configurar autenticación y nombre de usuario.
7. Guardar.

No utilizar un servidor ficticio ni reutilizar credenciales de un proveedor diferente.

## 6. Ejemplo técnico: Brevo

Brevo documenta los siguientes parámetros para su servicio SMTP transaccional:

| Parámetro | Valor de referencia |
|---|---|
| Servidor | `smtp-relay.brevo.com` |
| Puerto | `587` |
| Seguridad | STARTTLS |
| Autenticación | Contraseña normal con conexión cifrada |
| Usuario | Login SMTP asignado por Brevo |
| Contraseña | Clave SMTP, no clave API |

Consultar siempre los parámetros vigentes en la cuenta del proveedor.

El login SMTP puede ser distinto del correo que aparece como remitente.

**Restricción importante:** el servicio está documentado para mensajes transaccionales. Su uso para correspondencia profesional individual no debe considerarse autorizado sin una confirmación aplicable del proveedor.

La implementación de JH-Web demuestra la conexión técnica, no una autorización contractual universal.

## 7. Crear una identidad adicional

En Thunderbird:

1. Abrir la configuración de la cuenta receptora.
2. Seleccionar Manage Identities / Administrar identidades.
3. Elegir Add / Añadir.
4. Especificar un nombre visible.
5. Introducir la dirección profesional autorizada.
6. Seleccionar el servidor SMTP correspondiente.
7. Revisar firma y dirección de respuesta.
8. Guardar.

Ejemplo ilustrativo:

- Nombre: Professional Contact.
- Dirección: `contact@example.com`.
- Servidor: el SMTP autorizado configurado previamente.

No modificar el SMTP de la identidad personal si ambas necesitan servidores diferentes.

## 8. Protección de credenciales

- Crear claves específicas cuando el proveedor lo permita.
- No utilizar claves API como contraseñas SMTP.
- No guardar secretos en GitHub, documentos públicos ni capturas.
- Utilizar conexiones cifradas.
- Revocar claves comprometidas.
- Revisar expiración y renovación.
- Proteger el perfil local de Thunderbird.

Nunca publicar capturas de claves, tokens ni pantallas con secretos visibles.

## 9. Primera prueba de envío

1. Crear un mensaje nuevo.
2. Seleccionar explícitamente la identidad profesional.
3. Utilizar un destinatario externo bajo control propio.
4. Enviar un mensaje de prueba con asunto identificable.
5. Confirmar su recepción.
6. Comprobar el campo From.
7. Revisar el encabezado Reply-To si está definido.
8. Responder desde el destinatario.
9. Confirmar que la respuesta llega al buzón esperado.

Evitar datos personales o confidenciales en los mensajes de prueba.

## 10. Autenticación y cabeceras

El servidor SMTP puede autenticar al usuario correctamente sin que el dominio remitente supere SPF, DKIM o DMARC.

Conviene inspeccionar los resultados del mensaje recibido:

- SPF comprueba el dominio del remitente de sobre.
- DKIM valida la firma criptográfica.
- DMARC requiere alineación del dominio visible con SPF o DKIM.

En una prueba de JH-Web, Outlook informó SPF PASS para el dominio de retorno de Brevo, DKIM PASS para el dominio profesional y DMARC PASS mediante alineación DKIM.

Este resultado pertenece a esa prueba y no garantiza la recepción futura.

La Guía 06 profundizará en estos mecanismos.

## 11. Problemas habituales

| Síntoma | Comprobación |
|---|---|
| Fallo de autenticación | Login SMTP, clave y estado del servicio |
| Error TLS | Puerto y modo STARTTLS/SSL indicados |
| Remitente rechazado | Dominio o dirección autorizados |
| Identidad incorrecta | Servidor SMTP asignado a la identidad |
| Mensaje no entregado | Rebotes, límites, políticas y registros del proveedor |
| Respuesta a otro buzón | From y Reply-To |
| Mensaje marcado como spam | Autenticación, reputación y contenido |
| No aparecen enviados | Configuración de copias y carpetas |

Un mensaje aceptado por el SMTP no equivale necesariamente a un mensaje entregado en la bandeja de entrada.

## 12. Privacidad y seguimiento

Algunos proveedores de correo transaccional ofrecen seguimiento de aperturas, enlaces o eventos.

Comprobar estas funciones antes de enviar correspondencia profesional, especialmente cuando existan obligaciones de privacidad.

Desactivar el seguimiento cuando no exista una necesidad legítima y revisar las condiciones del proveedor.

## 13. Limitaciones

Este modelo combina servicios independientes y puede requerir administración adicional.

No incluye automáticamente:

- Buzones compartidos.
- Retención empresarial centralizada.
- Garantías contractuales de disponibilidad.
- Archivado o descubrimiento electrónico.
- Soporte empresarial integrado.

Cuando esas funciones sean necesarias, valorar una plataforma de correo profesional completa.

## 14. Resultado esperado

Thunderbird permite seleccionar una identidad profesional y transmitir mensajes mediante un SMTP autorizado, manteniendo separadas las configuraciones de envío y recepción.

## 15. Referencias

- [Mozilla Thunderbird — Configuración manual](https://support.mozilla.org/en-US/kb/manual-account-configuration)
- [Mozilla Thunderbird — Configuración de identidades](https://support.mozilla.org/en-US/kb/configuration-options-identity)
- [Brevo — SMTP transaccional](https://help.brevo.com/hc/es/articles/7924908994450-Enviar-emails-transaccionales-con-Brevo-SMTP)
- [Brevo — Claves SMTP](https://help.brevo.com/hc/es/articles/7959631848850-Crear-y-administrar-claves-SMTP)
- [Implementación JH-Web](https://juanherbozo.com/es/projects/jh-web-personal-digital-platform/)