# Guía 03 — Dominio y configuración DNS

[Inicio](../../../README.es.md) | [Arquitectura](../architecture.md) | [English](../../en/guides/03-domain-dns.md)

## 1. Objetivo

Conectar un dominio personalizado a una web alojada en Cloudflare Pages y comprender los registros DNS necesarios sin interrumpir servicios existentes.

Esta guía utiliza nombres de ejemplo. No proporciona valores DNS universales para copiar directamente a producción.

## 2. Registrador y proveedor DNS

El registrador administra la inscripción y renovación del dominio.

El proveedor DNS autoritativo administra los registros que utilizan los servicios asociados al dominio.

Ambas funciones pueden corresponder a empresas diferentes.

## 3. Registros DNS fundamentales

| Registro | Función |
|---|---|
| A | Asocia un nombre con una dirección IPv4 |
| AAAA | Asocia un nombre con una dirección IPv6 |
| CNAME | Define un alias hacia otro nombre DNS |
| MX | Identifica servidores de recepción de correo |
| TXT | Publica información como verificaciones y políticas SPF o DMARC |
| CAA | Restringe qué autoridades pueden emitir certificados |
| NS | Identifica servidores DNS autoritativos o delegaciones |

DKIM puede publicarse mediante TXT o CNAME según el proveedor.

## 4. Inventario inicial

Antes de modificar los DNS:

1. Identificar el registrador.
2. Confirmar los servidores de nombres actuales.
3. Exportar o documentar todos los registros.
4. Identificar servicios web y de correo existentes.
5. Revisar DNSSEC si está habilitado.
6. Preparar un procedimiento de recuperación.

Un cambio de servidores de nombres requiere especial precaución, ya que puede afectar simultáneamente varios servicios.

## 5. Conectar Cloudflare Pages

Primero se necesita un proyecto Pages publicado correctamente.

En Cloudflare:

1. Abrir Workers & Pages.
2. Seleccionar el proyecto.
3. Acceder a Custom domains.
4. Seleccionar Set up a domain.
5. Introducir el dominio deseado.
6. Seguir las instrucciones de validación.

Para utilizar el dominio raíz con Pages, la zona debe estar configurada en la cuenta de Cloudflare correspondiente.

Para determinados subdominios es posible utilizar DNS externo mediante un CNAME, pero el dominio también debe asociarse desde la configuración del proyecto Pages.

No basta con crear un registro manual y omitir la asociación del dominio personalizado.

## 6. Registros y proxy

Los registros A, AAAA y CNAME destinados a tráfico web pueden utilizar el proxy HTTP de Cloudflare.

Los registros MX y TXT no se proxifican.

Los nombres de host utilizados exclusivamente por servicios SMTP, IMAP o POP3 deben resolver sin el proxy HTTP estándar.

Los CNAME de verificación de servicios de terceros normalmente deben configurarse como DNS only, de acuerdo con las instrucciones del proveedor.

## 7. Dominio raíz y CNAME flattening

Cloudflare permite utilizar CNAME flattening en el dominio raíz.

Esta función facilita la resolución de nombres aunque el dominio raíz normalmente no permita un CNAME convencional.

No se deben crear direcciones IP ficticias para conectar Cloudflare Pages.

## 8. Coexistencia con correo

Una web y un servicio de correo pueden compartir dominio, pero utilizan registros y servicios diferentes.

Antes de cambiar registros MX:

- Identificar el proveedor actual de recepción.
- Confirmar si existe Cloudflare Email Routing.
- Comprobar las prioridades y destinos configurados.
- Evitar mezclar configuraciones incompatibles.

Los registros SPF, DKIM y DMARC se explicarán en una guía específica.

## 9. Validaciones DNS

En Windows PowerShell:

```powershell
Resolve-DnsName example.com -Type NS
Resolve-DnsName example.com -Type A
Resolve-DnsName example.com -Type MX
Resolve-DnsName example.com -Type TXT
```

Sustituir `example.com` por el dominio que corresponda.

Un resultado DNS correcto no demuestra por sí solo que la aplicación web o el servicio de correo funcionen correctamente.

## 10. Validación de la web

Después de asociar el dominio:

- Revisar el estado del dominio personalizado en Pages.
- Comprobar el acceso mediante HTTPS.
- Validar las rutas principales.
- Verificar el certificado TLS.
- Comprobar redirecciones entre dominio raíz y `www`, si existen.
- Confirmar que los servicios de correo siguen funcionando.

## 11. Errores frecuentes

| Problema | Revisión |
|---|---|
| Dominio no activo | Revisar servidores de nombres y estado en Pages |
| Error de asociación | Confirmar el alta en Custom domains |
| Certificado pendiente | Revisar validación, DNS y restricciones CAA |
| Correo interrumpido | Revisar MX y nombres de host relacionados |
| Verificación DKIM fallida | Revisar CNAME, proxy y flattening |
| Respuestas DNS diferentes | Comprobar TTL y cachés DNS |

## 12. Seguridad y continuidad

No modificar los registros de producción sin conocer los servicios afectados.

Restringir los accesos administrativos, habilitar MFA, conservar un inventario de configuraciones y planificar los cambios de DNS con posibilidad de recuperación.

## 13. Referencias

- [Cloudflare Pages: dominios personalizados](https://developers.cloudflare.com/pages/configuration/custom-domains/)
- [Cloudflare DNS: registros](https://developers.cloudflare.com/dns/manage-dns-records/)
- [Cloudflare: CNAME flattening](https://developers.cloudflare.com/dns/cname-flattening/)
- [Cloudflare: problemas de correo](https://developers.cloudflare.com/dns/troubleshooting/email-issues/)
- [Implementación JH-Web](https://juanherbozo.com/es/projects/jh-web-personal-digital-platform/)