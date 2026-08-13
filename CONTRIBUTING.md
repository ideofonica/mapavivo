# CONTRIBUTING.md

# 🤝 Contribuir a Mapa Vivo Colombia

Gracias por tu interés en contribuir a **Mapa Vivo Colombia**.

Mapa Vivo Colombia es un proyecto comunitario de código abierto orientado a facilitar la visualización y comunicación colaborativa de información durante situaciones de emergencia.

Las contribuciones son bienvenidas, pero debido a la naturaleza del proyecto, cualquier cambio debe priorizar:

- seguridad;
- privacidad;
- confiabilidad de la información;
- accesibilidad;
- funcionamiento en dispositivos móviles;
- funcionamiento con conectividad limitada;
- claridad para usuarios en situaciones de emergencia.

---

# 📌 Antes de contribuir

Antes de comenzar:

1. Lee el `README.md`.
2. Revisa los Issues existentes.
3. Comprueba que tu propuesta no haya sido planteada anteriormente.
4. No introduzcas información falsa o datos de prueba en producción.
5. Nunca subas credenciales privadas al repositorio.
6. Si el cambio afecta la base de datos, revisa primero las políticas RLS.
7. Si el cambio afecta información de emergencia, explica claramente cómo se valida.

---

# 🧭 Tipos de contribuciones

No necesitas ser desarrollador para contribuir.

## 💻 Código

Puedes contribuir con:

- JavaScript;
- HTML;
- CSS;
- PWA;
- IndexedDB;
- Service Workers;
- Supabase;
- PostgreSQL;
- PostGIS;
- optimización móvil;
- accesibilidad;
- seguridad;
- visualización geográfica.

---

## 🗺️ Mapas y datos geográficos

Son especialmente útiles las contribuciones relacionadas con:

- PostGIS;
- consultas espaciales;
- geolocalización;
- clustering;
- mapas de afectación;
- cálculo de distancias;
- visualización territorial;
- optimización de mapas para dispositivos móviles.

---

## 📡 Fuentes de información

Puedes proponer nuevas fuentes públicas y verificables.

Toda fuente debe documentar:

```text
Nombre de la fuente
URL
Tipo de información
Frecuencia de actualización
Método de acceso
Licencia o condiciones de uso
Responsable de la fuente

No se deben incorporar datos de procedencia desconocida como información oficial.

🚨 Información de emergencia

Esta es una regla fundamental del proyecto.

Nunca crear, modificar o importar datos de emergencia sin conocer su procedencia.

Por ejemplo, no se deben introducir manualmente como datos reales:

hospitales inexistentes;
refugios ficticios;
terremotos ficticios;
vías bloqueadas sin fuente;
personas afectadas inventadas;
coordenadas inventadas;
recursos inexistentes.

Los datos de prueba deben estar claramente identificados como:

TEST
DEMO
MOCK

y nunca mezclarse con datos de producción.

🔐 Seguridad

La seguridad es una prioridad.

Nunca subir al repositorio:

service_role keys
private keys
passwords
tokens privados
credenciales administrativas
API keys secretas
archivos .env con secretos

Si necesitas variables de entorno, utiliza:

.env.example

Ejemplo:

SUPABASE_URL=
SUPABASE_ANON_KEY=

Nunca:

SUPABASE_SERVICE_ROLE_KEY=xxxxxxxx
🛡️ Supabase

El proyecto utiliza:

Supabase Auth;
PostgreSQL;
PostGIS;
Row Level Security;
RPC;
Storage.

Cualquier modificación de la base de datos debe considerar las políticas RLS existentes.

No se debe solucionar un problema de frontend desactivando RLS.

Nunca hacer esto:
ALTER TABLE reports DISABLE ROW LEVEL SECURITY;

para solucionar un problema de desarrollo.

Si una operación necesita permisos adicionales, debe analizarse y diseñarse una política específica.

👤 Identidad de usuarios

La aplicación utiliza Supabase Authentication.

No se debe crear un sistema paralelo de identidad utilizando:

localStorage

como sustituto de:

auth.uid()

Los identificadores de usuario utilizados para operaciones protegidas deben provenir de Supabase Auth.

🤝 Corroboraciones

Las corroboraciones comunitarias utilizan la función RPC:

increment_report_verification()

No se debe incrementar directamente:

verification_count

desde el frontend.

La arquitectura correcta es:

Usuario
   ↓
RPC
   ↓
validación
   ↓
corroboración
   ↓
COUNT real
   ↓
actualización del reporte

No modificar este mecanismo sin analizar primero las condiciones de concurrencia y seguridad.

📴 Funcionamiento offline

La aplicación está diseñada para funcionar parcialmente sin conexión.

Los cambios relacionados con:

IndexedDB;
Service Worker;
Cache API;
sync queue;
sincronización;

deben probarse tanto:

ONLINE

como:

OFFLINE

Una funcionalidad no debe considerarse terminada únicamente porque funciona con conexión.

📱 Desarrollo móvil

La mayoría de los usuarios pueden acceder desde teléfonos.

Por ello, cualquier modificación de interfaz debe probarse al menos en:

pantalla pequeña;
pantalla mediana;
conexión lenta;
orientación vertical;
orientación horizontal cuando corresponda.

Debe evitarse introducir procesos pesados que bloqueen la interfaz.

🧪 Datos de prueba

Para pruebas locales puedes utilizar datos ficticios.

Sin embargo, deben identificarse claramente.

Ejemplo:

{
    test: true,
    source_type: "demo"
}

Nunca presentar datos de prueba como:

OFICIAL
VERIFICADO
SGC
HOSPITAL
REFUGIO

si realmente no provienen de esas fuentes.

🌿 Flujo de trabajo recomendado
1. Fork

Haz un fork del repositorio.

2. Clona tu fork
git clone https://github.com/TU-USUARIO/mapa-vivo-colombia.git

cd mapa-vivo-colombia
3. Crea una rama

No trabajes directamente sobre main.

git checkout -b feature/nombre-de-la-funcion

Ejemplos:

git checkout -b feature/filtro-reportes
git checkout -b fix/offline-sync
git checkout -b improvement/mobile-map
🧩 Convención de ramas

Se recomienda:

feature/     nuevas funcionalidades
fix/         corrección de errores
security/    mejoras de seguridad
docs/        documentación
performance/ optimización
accessibility/ accesibilidad
data/        fuentes y procesamiento de datos

Ejemplos:

feature/report-filter
fix/indexeddb-sync
security/storage-policy
docs/api-documentation
performance/map-rendering
accessibility/high-contrast
📝 Commits

Procura que los commits describan claramente el cambio.

Recomendado
git commit -m "Añade filtro por tipo de reporte"
git commit -m "Corrige sincronización offline"
git commit -m "Mejora accesibilidad del mapa"
Evitar
git commit -m "cambios"
git commit -m "fix"
git commit -m "cosas"
🧪 Antes de crear un Pull Request

Comprueba:

Funcionalidad
 La función funciona.
 No rompe funciones existentes.
 Funciona en móvil.
 Funciona con conexión lenta.
 Funciona correctamente sin conexión cuando corresponde.
Seguridad
 No hay credenciales privadas.
 No se desactivó RLS.
 No se exponen datos privados.
 No se permiten modificaciones no autorizadas.
 No se introdujeron nuevas rutas inseguras.
Datos
 Los datos tienen una fuente identificable.
 Los datos oficiales están diferenciados de los comunitarios.
 No hay información ficticia presentada como real.
 Las coordenadas fueron verificadas.
Código
 No hay errores en consola.
 No hay código innecesario.
 Se eliminaron logs de depuración innecesarios.
 Se actualizaron los comentarios relevantes.
📤 Pull Request

Cuando tu cambio esté listo:

git push origin feature/nombre-de-la-funcion

Después abre un Pull Request hacia:

main
📋 Formato recomendado para Pull Requests

Utiliza una descripción similar a:

## ¿Qué cambia?

Describe brevemente el cambio.

## ¿Por qué?

Explica el problema que resuelve.

## ¿Cómo se probó?

Describe las pruebas realizadas.

## ¿Afecta la base de datos?

- [ ] Sí
- [ ] No

## ¿Afecta seguridad/RLS?

- [ ] Sí
- [ ] No

## ¿Afecta funcionamiento offline?

- [ ] Sí
- [ ] No

## ¿Afecta dispositivos móviles?

- [ ] Sí
- [ ] No

## Capturas

Añade capturas cuando el cambio sea visual.
🚨 Cambios de alto riesgo

Algunos cambios requieren especial revisión.

Por ejemplo:

cambios en RLS;
cambios en Authentication;
cambios en Storage;
modificaciones de RPC;
modificaciones de reports;
modificaciones de corroboraciones;
importación de fuentes externas;
cambios en sincronización offline;
cambios que puedan alterar información de emergencia.

Estos Pull Requests deben explicar:

qué cambia;
por qué;
qué riesgo introduce;
cómo se probó;
cómo puede revertirse.
🐛 Reportar errores

Para reportar un error, abre un Issue.

Incluye:

Título:
Descripción:
Pasos para reproducir:
Resultado esperado:
Resultado obtenido:
Dispositivo:
Navegador:
Conexión:
Capturas:
Consola:

Ejemplo:

## Error

Los reportes creados offline no se sincronizan al recuperar conexión.

## Pasos

1. Abrir la aplicación.
2. Activar modo avión.
3. Crear reporte.
4. Cerrar modo avión.
5. Esperar sincronización.

## Resultado esperado

El reporte aparece en Supabase.

## Resultado actual

El reporte permanece en sync_queue.

## Dispositivo

Android / Chrome.
🔒 Reportar vulnerabilidades de seguridad

No publiques vulnerabilidades de seguridad sensibles como Issues públicos.

Si encuentras:

exposición de datos;
bypass de RLS;
acceso a reportes privados;
acceso a archivos de otros usuarios;
exposición de credenciales;
posibilidad de modificar información ajena;
vulnerabilidades de autenticación;

contacta primero con los mantenedores del proyecto mediante el canal privado indicado en el repositorio.

No publiques exploits funcionales antes de que la vulnerabilidad haya sido revisada.

🌎 Filosofía del proyecto

Mapa Vivo Colombia busca construir una herramienta tecnológica que pueda ser utilizada por comunidades durante situaciones de emergencia.

Por eso:

La confiabilidad es más importante que la cantidad de funcionalidades.

Una función que parece pequeña pero introduce información incorrecta puede ser más perjudicial que no tener esa función.

🤝 Código abierto y comunidad

Las contribuciones pueden venir de:

desarrolladores;
diseñadores;
investigadores;
geógrafos;
especialistas en emergencias;
expertos en UX;
especialistas en accesibilidad;
organizaciones comunitarias;
instituciones;
ciudadanos.

No es necesario contribuir únicamente con código.

También son valiosas:

documentación;
pruebas;
traducciones;
accesibilidad;
investigación;
fuentes de datos;
análisis territorial;
diseño;
propuestas de funcionalidades.
📜 Licencia

Este proyecto se distribuye bajo:

MIT License

Consulta:

LICENSE

para conocer las condiciones completas.

❤️ Gracias

Cada contribución puede ayudar a mejorar una herramienta cuyo objetivo es facilitar el acceso comunitario a información durante situaciones críticas.

Gracias por contribuir a Mapa Vivo Colombia.
