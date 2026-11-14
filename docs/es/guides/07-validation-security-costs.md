# Guía 07 — Pruebas, seguridad, costes y limitaciones

[Inicio](../../../README.es.md) | [Arquitectura](../architecture.md) | [English](../../en/guides/07-validation-security-costs.md)

## 1. Objetivo

Evaluar una infraestructura digital profesional antes de considerarla lista para utilizarse.

Esta guía complementa las instrucciones de implementación. No constituye una auditoría de seguridad, certificación de cumplimiento ni garantía de disponibilidad.

## 2. Alcance de las pruebas

La evaluación cubre:

- Publicación y funcionamiento de la web.
- Resolución DNS y HTTPS.
- Recepción y envío de correo.
- Autenticación SPF, DKIM y DMARC.
- Gestión de credenciales y accesos.
- Dependencias y costes operativos.
- Recuperación ante errores.

Los resultados deben registrarse con fecha, entorno y evidencia verificable, sin divulgar información privada.

## 3. Matriz de validación

| ID | Comprobación | Resultado esperado |
|---|---|---|
| WEB-01 | Compilación local | Finaliza sin errores |
| WEB-02 | Despliegue | Versión prevista publicada |
| WEB-03 | Navegación | Rutas principales accesibles |
| WEB-04 | Idiomas | Contenido y enlaces correctos |
| DNS-01 | Dominio | Resolución coherente |
| DNS-02 | HTTPS | Certificado válido |
| MAIL-01 | Recepción | Mensaje externo recibido |
| MAIL-02 | Envío | Mensaje de prueba aceptado y recibido |
| MAIL-03 | Respuesta | Llega al buzón esperado |
| AUTH-01 | SPF | Resultado e identidad documentados |
| AUTH-02 | DKIM | Firma y dominio comprobados |
| AUTH-03 | DMARC | Resultado y alineación comprobados |

No marcar una prueba como superada sin ejecutarla.

## 4. Pruebas web

Verificar:

1. Compilación local.
2. Despliegue y commit publicado.
3. Acceso HTTPS.
4. Página inicial y rutas internas.
5. Imágenes y recursos.
6. Navegación móvil.
7. Metadatos e idiomas.
8. Ausencia de información confidencial expuesta.

Una compilación correcta no demuestra por sí sola que todas las páginas funcionen en producción.

## 5. Pruebas DNS

En Windows PowerShell:

```powershell
Resolve-DnsName example.com -Type NS
Resolve-DnsName example.com -Type MX
Resolve-DnsName example.com -Type TXT
Resolve-DnsName _dmarc.example.com -Type TXT
```

Adaptar el dominio. Registrar los valores esperados y comprobar que los cambios no afectan a otros servicios.

## 6. Pruebas de correo

Realizar al menos:

1. Envío desde un proveedor externo a la dirección profesional.
2. Confirmación de reenvío al buzón de destino.
3. Envío desde la identidad profesional mediante SMTP autorizado.
4. Recepción en otro proveedor.
5. Respuesta y comprobación de su destino.
6. Revisión de errores, rebotes y carpetas de spam.
7. Inspección de los encabezados de autenticación.

La aceptación del SMTP no garantiza entrega en bandeja de entrada.

## 7. Evidencia de JH-Web

En la implementación de referencia se comprobó:

- Publicación de páginas estáticas bilingües.
- Despliegue de la web mediante GitHub y Cloudflare Pages.
- Recepción por Cloudflare Email Routing en Gmail.
- Envío mediante Thunderbird y Brevo SMTP.
- Recepción de un mensaje de prueba en Outlook.
- Respuesta a la dirección profesional.
- SPF PASS para el dominio de retorno del proveedor.
- DKIM PASS con el dominio profesional.
- DMARC PASS mediante alineación DKIM.

Estas observaciones corresponden a pruebas concretas. No representan un acuerdo de nivel de servicio ni una evaluación de todos los escenarios posibles.

El uso de Brevo SMTP para correspondencia profesional individual sigue sujeto a la confirmación de sus condiciones aplicables.

## 8. Seguridad y privacidad

Revisar:

- MFA en GitHub, Cloudflare y cuentas de correo.
- Credenciales únicas y gestor de contraseñas.
- Tokens y claves fuera de repositorios.
- Permisos mínimos para integraciones.
- Renovación y revocación de claves.
- Actualizaciones de Thunderbird y dependencias.
- Protección del equipo local.
- Privacidad y condiciones de tratamiento de datos.
- Funciones de seguimiento de aperturas y enlaces del proveedor SMTP.

La configuración de SPF, DKIM y DMARC no sustituye la protección de las cuentas ni el tratamiento adecuado de información sensible.

## 9. Coste total

Separar gastos monetarios y esfuerzo de administración.

| Concepto | Evaluación |
|---|---|
| Dominio | Registro y renovación |
| Alojamiento | Límites y condiciones del plan |
| DNS y reenvío | Límites y dependencia del proveedor |
| SMTP | Uso permitido, cuotas y posible contratación |
| Mantenimiento | Tiempo de soporte y actualizaciones |
| Continuidad | Copias, recuperación y migración |
| Escalado | Servicios y capacidades futuras |

Los planes gratuitos pueden reducir el gasto recurrente, pero no eliminan el coste total de propiedad.

No declarar un importe mensual universal: los precios y límites cambian.

## 10. Riesgos y medidas

| Riesgo | Medida |
|---|---|
| Cuenta comprometida | MFA y revisión de accesos |
| Clave SMTP expuesta | Revocación y sustitución |
| DNS incorrecto | Inventario y procedimiento de reversión |
| Despliegue defectuoso | Validación y recuperación de versión |
| Fallo de reenvío | Supervisión y alternativa de contacto |
| Cambio de condiciones | Revisión periódica del proveedor |
| Dependencia de varios servicios | Documentación de responsables y recuperación |

Mantener una copia recuperable del contenido y conocer el procedimiento para restaurar configuraciones importantes.

## 11. Cuándo considerar esta arquitectura

Puede ser razonable para:

- Sitios informativos estáticos.
- Portafolios profesionales.
- Proyectos pequeños con mantenimiento técnico disponible.
- Escenarios donde las limitaciones de correo y alojamiento sean aceptables.

Conviene evaluar una solución empresarial completa cuando se necesiten:

- Buzones compartidos o administración centralizada.
- Garantías contractuales de disponibilidad.
- Retención avanzada, auditoría o cumplimiento específico.
- Soporte con compromisos de respuesta.
- Procesos críticos o información especialmente sensible.

La elección debe basarse en requisitos, no únicamente en el ahorro inicial.

## 12. Lista de aceptación

- [ ] Web publicada y validada.
- [ ] Dominio y certificado comprobados.
- [ ] Registros DNS documentados.
- [ ] Correo entrante probado.
- [ ] Correo saliente probado y autorizado.
- [ ] Respuestas recibidas correctamente.
- [ ] SPF, DKIM y DMARC revisados.
- [ ] MFA y permisos evaluados.
- [ ] Credenciales protegidas.
- [ ] Costes y límites documentados.
- [ ] Procedimiento de recuperación definido.
- [ ] Riesgos aceptados por el responsable.

## 13. Conclusión

Una infraestructura sencilla puede ser suficiente cuando sus requisitos están bien definidos y sus limitaciones son conocidas.

La calidad de la solución no depende del número de servicios contratados, sino de su adecuación al propósito, su seguridad, su capacidad de mantenimiento y la evidencia de funcionamiento.

## 14. Referencias

- [Cloudflare Pages](https://developers.cloudflare.com/pages/)
- [Cloudflare Email Routing](https://developers.cloudflare.com/email-service/)
- [Astro Documentation](https://docs.astro.build/)
- [RFC 7489 — DMARC](https://www.rfc-editor.org/rfc/rfc7489)
- [Implementación JH-Web](https://juanherbozo.com/es/projects/jh-web-personal-digital-platform/)