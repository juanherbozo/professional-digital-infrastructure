# Guía 01 — Preparación y requisitos

[Inicio](../../../README.es.md) | [Arquitectura](../architecture.md) | [English](../../en/guides/01-prerequisites.md)

## 1. Objetivo

Esta guía presenta los requisitos y las decisiones necesarias antes de implementar una presencia digital profesional basada en tecnologías web estáticas y servicios cloud independientes.

La arquitectura de referencia es JH-Web — Personal Digital Platform.

No es necesario contratar todos los servicios antes de comenzar. La preparación debe adaptarse a las necesidades del proyecto.

## 2. Definir el alcance

Antes de elegir tecnologías, identificar:

- Público destinatario de la web.
- Contenidos y funcionalidades necesarias.
- Idiomas que se publicarán.
- Necesidad de formularios o servicios dinámicos.
- Requisitos de correo profesional.
- Presupuesto disponible.
- Capacidad para administrar la solución.

Una web informativa no tiene necesariamente los mismos requisitos que una tienda online o una aplicación empresarial.

## 3. Herramientas de desarrollo

La implementación de referencia utiliza:

| Herramienta | Función |
|---|---|
| Editor de código | Modificar el proyecto |
| Node.js y npm | Gestionar dependencias y ejecutar Astro |
| Astro | Generar la web estática |
| TypeScript | Desarrollar componentes y lógica |
| Markdown | Administrar contenidos |
| Git | Controlar versiones |
| GitHub | Alojar el repositorio |

Consultar las versiones compatibles en la documentación oficial de cada herramienta antes de instalar.

### Comprobaciones iniciales

En una terminal:

```powershell
node --version
npm --version
git --version
```

Las versiones deben ser compatibles con el proyecto que se vaya a implementar.

## 4. Servicios cloud

| Servicio | Función en la arquitectura |
|---|---|
| GitHub | Repositorio y colaboración |
| Cloudflare Pages | Publicación de la web |
| Cloudflare DNS | Gestión del dominio |
| Cloudflare Email Routing | Reenvío de correo entrante |
| Gmail | Buzón de destino verificado |
| Brevo SMTP | Transporte de correo saliente |
| Thunderbird | Cliente de correo |

No todos los servicios son obligatorios para publicar una web.

La recepción y el envío de correo constituyen componentes independientes y requieren una evaluación adicional.

## 5. Dominio

Se recomienda disponer de un dominio propio que represente la identidad profesional.

Antes de configurarlo:

1. Confirmar su titularidad.
2. Identificar al registrador.
3. Revisar la renovación y su coste.
4. Determinar dónde se administrarán los DNS.
5. Inventariar los registros existentes.
6. Evaluar el impacto de cualquier cambio sobre servicios activos.

No sustituir registros MX, SPF, DKIM o DMARC sin comprender sus efectos.

## 6. Decisiones de correo

La configuración de referencia separa dos funciones:

- Recepción: Cloudflare Email Routing reenvía mensajes a Gmail.
- Envío: Thunderbird utiliza un servidor SMTP externo.

Estas funciones no equivalen a contratar un buzón empresarial completo.

Antes de adoptarlas, comprobar las condiciones de uso, la protección de datos, los límites operativos y las necesidades de soporte.

El uso de Brevo SMTP para correspondencia profesional individual requiere confirmación de compatibilidad con sus condiciones de servicio.

## 7. Seguridad previa

Se recomienda:

- Habilitar autenticación multifactor cuando esté disponible.
- Utilizar contraseñas únicas y un gestor de credenciales.
- No almacenar claves SMTP ni tokens en el repositorio.
- Revisar los permisos de GitHub y los proveedores.
- Proteger el equipo de desarrollo.
- Planificar la renovación y revocación de credenciales.
- Evitar publicar capturas con información privada.

Los registros SPF, DKIM y DMARC se abordarán en una guía independiente.

## 8. Evaluación económica

Los planes gratuitos pueden reducir los gastos recurrentes, pero no eliminan el coste del dominio, la administración ni el mantenimiento.

También deben evaluarse:

- Límites de uso.
- Coste de ampliación.
- Cambios contractuales.
- Tiempo de soporte.
- Riesgo de interrupciones.
- Dependencia de proveedores.

## 9. Lista de preparación

Antes de comenzar la implementación, verificar:

- [ ] Alcance funcional definido.
- [ ] Dominio disponible o registrado.
- [ ] Acceso administrativo a los DNS.
- [ ] Entorno local preparado.
- [ ] Cuenta GitHub disponible.
- [ ] Requisitos de correo identificados.
- [ ] Condiciones de proveedores revisadas.
- [ ] Credenciales protegidas.
- [ ] Presupuesto y mantenimiento considerados.

## 10. Siguiente etapa

La siguiente guía abordará la construcción y publicación de una web estática mediante Astro, GitHub y Cloudflare Pages.

[Consultar la implementación real de JH-Web](https://juanherbozo.com/es/projects/jh-web-personal-digital-platform/)