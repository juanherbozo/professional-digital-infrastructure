# Guía 06 — SPF, DKIM y DMARC

[Inicio](../../../README.es.md) | [Arquitectura](../architecture.md) | [English](../../en/guides/06-spf-dkim-dmarc.md)

## 1. Objetivo

Comprender y validar la autenticación del correo electrónico enviado desde un dominio profesional.

Los mecanismos analizados son SPF, DKIM y DMARC. Esta guía utiliza dominios ilustrativos; no proporciona registros que deban copiarse sin adaptación.

## 2. Tres mecanismos diferentes

| Mecanismo | Verificación |
|---|---|
| SPF | Autoriza los servidores que pueden enviar utilizando un dominio de sobre SMTP |
| DKIM | Comprueba una firma criptográfica asociada a un dominio |
| DMARC | Evalúa la alineación de SPF o DKIM con el dominio visible en From y publica una política |

Estos controles contribuyen a proteger contra suplantaciones, pero no garantizan la entrega en bandeja de entrada.

## 3. Identidades de un mensaje

Un mensaje puede contener varios dominios:

- Header From (RFC 5322): dirección visible para el usuario.
- Envelope sender / MAIL FROM: identidad SMTP usada habitualmente para SPF.
- DKIM d=: dominio responsable de la firma criptográfica.

Estos dominios no tienen por qué ser idénticos.

Por ello, un resultado SPF PASS no demuestra por sí solo que el dominio visible esté autenticado mediante DMARC.

## 4. SPF

SPF publica una política TXT que define qué infraestructuras están autorizadas a enviar utilizando el dominio evaluado.

Ejemplo ilustrativo:

```dns
example.com TXT "v=spf1 include:_spf.example-provider.net ~all"
```

El dominio incluido es ficticio y no representa un proveedor real.

Reglas básicas:

1. Consultar las instrucciones del proveedor SMTP.
2. Comprobar el dominio de sobre utilizado.
3. Evitar múltiples registros TXT SPF independientes para el mismo nombre.
4. Revisar el límite de consultas DNS del estándar SPF.
5. No añadir proveedores que no se utilicen.

La política SPF del dominio raíz no siempre determina el resultado de SPF de un mensaje enviado por un proveedor externo que utiliza su propio dominio de retorno.

## 5. DKIM

DKIM utiliza una clave privada para firmar el mensaje y una clave pública publicada mediante DNS para verificarlo.

Una firma identifica el dominio mediante `d=` y un selector mediante `s=`.

Ejemplo:

```text
DKIM-Signature: ... d=example.com; s=mail2026; ...
```

El registro puede publicarse como TXT o mediante un CNAME delegado, según el proveedor.

Pasos generales:

1. Verificar el dominio remitente en el proveedor.
2. Obtener los registros DKIM indicados.
3. Publicarlos exactamente como se proporcionan.
4. Comprobar su resolución DNS.
5. Enviar un mensaje de prueba.
6. Confirmar DKIM PASS y el dominio `d=`.

No publicar claves privadas DKIM en repositorios.

## 6. DMARC

DMARC compara el dominio visible en From con los dominios que superan SPF y/o DKIM.

Para obtener DMARC PASS se necesita al menos una de estas condiciones:

- SPF PASS y dominio SPF alineado.
- DKIM PASS y dominio DKIM alineado.

No es obligatorio superar ambas vías.

### Políticas

| Política | Finalidad |
|---|---|
| p=none | Supervisión sin solicitar cuarentena o rechazo |
| p=quarantine | Solicitar tratamiento sospechoso de mensajes fallidos |
| p=reject | Solicitar rechazo de mensajes fallidos |

Ejemplo de supervisión:

```dns
_dmarc.example.com TXT "v=DMARC1; p=none"
```

No copiar políticas restrictivas sin inventariar previamente todos los flujos legítimos de envío.

El receptor conserva capacidad de decisión sobre el tratamiento final.

## 7. Alineación relajada y estricta

DMARC admite dos modalidades:

- Relajada: comparación de dominios organizativos.
- Estricta: coincidencia exacta de nombres DNS.

Por defecto, la alineación es relajada salvo configuración diferente mediante `aspf` o `adkim`.

Una firma DKIM válida de un dominio ajeno al remitente visible puede superar DKIM, pero no servir para superar DMARC.

## 8. Ejemplo de la implementación JH-Web

En una prueba real enviada desde Thunderbird mediante Brevo SMTP y recibida en Outlook se observó:

| Control | Resultado | Interpretación |
|---|---|---|
| SPF | PASS | Validación del dominio de retorno del proveedor |
| DKIM | PASS | Firma con el dominio profesional |
| DMARC | PASS | Alineación DKIM con el dominio visible |

La interpretación correcta es que DMARC se superó mediante DKIM.

No debe afirmarse que el SPF del proveedor estaba alineado con el dominio profesional únicamente por observar SPF PASS.

Esta es una validación puntual, no una garantía futura de entregabilidad.

## 9. Comprobaciones DNS

En Windows PowerShell:

```powershell
Resolve-DnsName example.com -Type TXT
Resolve-DnsName _dmarc.example.com -Type TXT
Resolve-DnsName mail2026._domainkey.example.com -Type TXT
```

Sustituir el dominio y selector por los valores reales.

Si DKIM utiliza CNAME, consultar también el tipo CNAME.

Los registros DNS pueden estar correctos aunque la firma o el envío no estén configurados adecuadamente.

## 10. Prueba de extremo a extremo

1. Enviar un mensaje desde la identidad profesional.
2. Recibirlo en un proveedor externo.
3. Abrir los encabezados completos.
4. Localizar `Authentication-Results`.
5. Anotar SPF, DKIM y DMARC.
6. Revisar los dominios asociados a cada comprobación.
7. Comprobar la alineación.
8. Registrar resultados y fecha sin publicar datos sensibles.

Comprobar también posibles rechazos y avisos de entrega.

## 11. Problemas habituales

| Síntoma | Posible causa |
|---|---|
| SPF FAIL | IP no autorizada o dominio SPF incorrecto |
| SPF PASS y DMARC FAIL | SPF no alineado y ausencia de DKIM alineado válido |
| DKIM FAIL | Firma alterada, configuración errónea o clave DNS incorrecta |
| DKIM PASS y DMARC FAIL | Firma válida de dominio no alineado |
| DMARC inexistente | Registro ausente o mal publicado |
| Correo en spam con DMARC PASS | Reputación, contenido u otros filtros |

El reenvío puede afectar SPF y algunas transformaciones de mensajes pueden afectar DKIM.

## 12. Seguridad y operación

- Inventariar todos los servicios que envían correo.
- Mantener solamente autorizaciones necesarias.
- Supervisar informes DMARC si se habilitan.
- Aumentar gradualmente las políticas restrictivas.
- Revisar DKIM después de migraciones.
- Proteger las credenciales SMTP.
- Documentar pruebas y cambios.

## 13. Resultado esperado

Capacidad para diferenciar resultados de autenticación, comprender la alineación y validar de forma reproducible los mensajes enviados desde un dominio profesional.

## 14. Referencias

- [RFC 7489 — DMARC](https://www.rfc-editor.org/rfc/rfc7489)
- [RFC 7208 — SPF](https://www.rfc-editor.org/rfc/rfc7208)
- [RFC 6376 — DKIM](https://www.rfc-editor.org/rfc/rfc6376)
- [Cloudflare — Email authentication](https://developers.cloudflare.com/email-service/concepts/email-authentication/)
- [JH-Web — Implementación de referencia](https://juanherbozo.com/es/projects/jh-web-personal-digital-platform/)