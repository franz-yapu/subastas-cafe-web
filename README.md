# Subasta de café — Web

Aplicación web de una **plataforma de subastas en línea de café de especialidad** con pujas en tiempo
real. Los compradores siguen la subasta en vivo y pujan desde el navegador (también desde el móvil); el
administrador gestiona subastas, lotes, vendedores, usuarios y la imagen de marca.

API: [`franz-yapu/backend-c`](https://github.com/franz-yapu/backend-c)

## Qué hace

**Para el comprador**
- Sala de subasta en vivo: precio actual por lote, pujas que llegan al instante por WebSocket, aviso
  cuando alguien supera su oferta y cuenta regresiva que se extiende si hay pujas al final.
- Panel con sus datos, pedidos y recorrido guiado (*tour*) la primera vez que entra.
- Registro, inicio de sesión y recuperación de contraseña.

**Para el administrador**
- Panel con indicadores y gráficas (Chart.js).
- Gestión de subastas, lotes de café, vendedores, compradores y transacciones.
- **Marca blanca**: colores, logo y nombre desde el panel. El contraste de los textos
  se calcula automáticamente (WCAG) para que cualquier paleta siga siendo legible.
- **Editor de traducciones**: cambia textos de la interfaz sin tocar código.

**Sitio público**
- Portada, subasta activa, ganadores, documentos, reglamento y "acerca de".

## Stack

Angular 19 (**SSR** con `@angular/ssr`) · TypeScript · PrimeNG 19 · Tailwind CSS ·
socket.io-client · ngx-translate (español/inglés) · Chart.js · Nginx · Docker

## Puesta en marcha

Requisitos: Node 20+ y la [API](https://github.com/franz-yapu/backend-c) corriendo.

```bash
npm install
npm start                # http://localhost:4200  → API en http://localhost:3000
```

La URL de la API y del WebSocket salen de `src/environments/` (`backend` y `Socket`), nunca van
escritas en los componentes. Hay una configuración por despliegue:

| Comando | Environment | Uso |
| --- | --- | --- |
| `npm start` | `environment.ts` | desarrollo local |
| `npm run buildlocal` | `environment.local.ts` | pruebas en red local |
| `npm run buildcafe` | `environment.cafe.ts` | despliegue alternativo |
| `npm run buildprod` | `environment.prod.ts` | despliegue de producción |

Ajusta las URLs de esos archivos a tu propio servidor antes de compilar.

### Producción (SSR)

```bash
npm run buildprod
npm run serve:ssr:frontend-angular
```

O con Docker (`Dockerfile` + `nginx.conf` incluidos).

## Estructura

```
src/app/
  core/       guard, interceptor, branding (contraste WCAG), tour guiado
  modules/
    external-home/   sitio público
    buyer/           sala de subasta, pedidos, datos
    home/            panel de administración
    navbar/
  project/    componentes, servicios, pipes y directivas compartidos
public/assets/i18n/  es.json · en.json
```

## Notas de diseño

- Rutas con *lazy loading*; las áreas `home` y `buyer` están protegidas por `authGuard`, y un `401`
  cierra la sesión y redirige al login.
- Todos los textos pasan por ngx-translate; nada de cadenas escritas a mano en las vistas.
- Accesibilidad y rendimiento en móvil revisados con axe-core y Lighthouse.

## Estado

Versión **1.0.1** (etiquetas `v1.0.0`, `v1.0.1`). Rama de trabajo actual: `main`.

## Licencia

Todos los derechos reservados © Franz Yapu. El código se publica para que pueda revisarse;
no se concede licencia de uso o redistribución sin autorización escrita.
