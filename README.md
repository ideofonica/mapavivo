# 🌎 MAPA VIVO COLOMBIA

### Plataforma comunitaria de información y reporte colaborativo ante emergencias

**Mapa Vivo Colombia** es una herramienta web comunitaria de código abierto diseñada para facilitar la comunicación y visualización colaborativa de situaciones de emergencia en Colombia.

Permite a las personas:

- 🆘 Reportar que necesitan ayuda.
- 🏚️ Reportar daños.
- 🚧 Reportar problemas en vías.
- 📍 Consultar recursos disponibles.
- 🤝 Corroborar reportes realizados por otras personas.
- 🗺️ Visualizar información georreferenciada en un mapa.
- 📡 Consultar información sísmica proveniente del Servicio Geológico Colombiano (SGC).
- 📱 Utilizar la aplicación desde dispositivos móviles.
- 📴 Crear reportes incluso cuando temporalmente no existe conexión a Internet.

---

## ⚠️ IMPORTANTE

**Mapa Vivo Colombia NO es un servicio oficial del Gobierno de Colombia ni reemplaza los servicios de emergencia.**

La plataforma es un proyecto comunitario y experimental de código abierto.

En una emergencia real, se deben utilizar también los canales oficiales de atención y emergencia disponibles en cada territorio.

La información publicada por usuarios puede ser incompleta, incorrecta, desactualizada o estar pendiente de verificación.

Los datos sísmicos identificados como provenientes del SGC deben entenderse como información consultada desde la fuente correspondiente y no como una interpretación oficial realizada por Mapa Vivo Colombia.

---

# 🎯 Objetivo

El objetivo de Mapa Vivo Colombia es crear una infraestructura digital comunitaria que permita compartir información georreferenciada durante situaciones de emergencia.

La idea central es sencilla:

> **Una persona reporta lo que está ocurriendo.  
> Otras personas pueden verlo.  
> La comunidad puede corroborarlo.  
> La información se transforma en un mapa colectivo de situación.**

La plataforma busca reducir la fragmentación de información durante emergencias y facilitar que las personas puedan visualizar rápidamente qué está ocurriendo alrededor de ellas.

---

# 🧭 ¿Cómo funciona?

## 1. Reporte

Una persona puede crear un reporte desde su teléfono.

Ejemplos:

- Necesito ayuda.
- Hay personas heridas.
- Hay daños estructurales.
- Una vía está bloqueada.
- Existe una situación de peligro.

El reporte puede incluir:

- ubicación geográfica;
- tipo de situación;
- descripción;
- cantidad de personas;
- personas heridas;
- nivel de daño;
- estado de la vía;
- necesidades;
- fotografía, cuando esté habilitada.

---

## 2. Geolocalización

Los reportes utilizan coordenadas geográficas.

La información espacial se almacena utilizando:

- PostgreSQL
- PostGIS
- geometrías `Point`
- sistema de referencia WGS84 / EPSG:4326

Esto permite realizar posteriormente consultas espaciales como:

- reportes cercanos;
- reportes dentro de un radio;
- concentración de incidentes;
- zonas afectadas;
- recursos próximos.

---

## 3. Corroboración comunitaria

Los usuarios pueden corroborar reportes realizados por otras personas.

Un reporte puede pasar de:

```text
REPORTED

a:

CORROBORATED

cuando alcanza el número definido de corroboraciones independientes.

Las corroboraciones son procesadas mediante una función RPC de PostgreSQL para evitar condiciones de carrera en el contador.

Un usuario:

no puede corroborar su propio reporte;
no puede corroborar dos veces el mismo reporte.
📡 Información sísmica

Mapa Vivo Colombia puede consultar información sísmica proveniente del:

Servicio Geológico Colombiano (SGC)

La plataforma no genera ni inventa eventos sísmicos.

Si la fuente oficial no está disponible, la aplicación debe indicar que la información temporalmente no está disponible.

No se utilizan datos sísmicos ficticios como información oficial.

📴 Funcionamiento offline

Uno de los objetivos principales del proyecto es mantener funciones esenciales incluso cuando existe una conexión inestable.

La aplicación utiliza almacenamiento local mediante:

IndexedDB

Los reportes creados sin conexión pueden quedar temporalmente almacenados en:

sync_queue

Cuando vuelve la conectividad:

Usuario
   ↓
IndexedDB
   ↓
sync_queue
   ↓
Supabase
   ↓
PostgreSQL

La aplicación debe marcar un reporte como sincronizado solamente después de recibir confirmación del servidor.

🏗️ Arquitectura
                    MAPA VIVO COLOMBIA
                            │
             ┌──────────────┴──────────────┐
             │                             │
          FRONTEND                       PWA
             │                             │
             │                    Service Worker
             │                             │
             ▼                             ▼
       MapLibre GL JS                 IndexedDB
             │                             │
             └──────────────┬──────────────┘
                            │
                            ▼
                         SUPABASE
                            │
              ┌─────────────┼─────────────┐
              │             │             │
              ▼             ▼             ▼
        PostgreSQL       Storage        Auth
              │
              ▼
           PostGIS
              │
       ┌──────┼──────┐
       │      │      │
    reports  resources earthquakes
       │
       ▼
report_confirmations
🔐 Seguridad

El proyecto utiliza:

Supabase Authentication.
Anonymous Authentication.
Row Level Security (RLS).
auth.uid().
PostgreSQL RPC.
restricciones de integridad.
claves foráneas.
índices espaciales PostGIS.
restricciones de coordenadas.
restricciones únicas para corroboraciones.

Los usuarios no deben poder modificar reportes pertenecientes a otras personas.

Las corroboraciones son procesadas mediante una función de servidor.

🗄️ Base de datos

Principales tablas:

reports

Contiene los reportes comunitarios.

Incluye información como:

id
type
latitude
longitude
geom
municipality
department
description
status
confidence
verification_count
user_id
created_at
updated_at
is_public
image_url
needs
injured_count
in_danger
people_count
damage_types
damage_level
road_status
location_detail
report_confirmations

Registra las corroboraciones:

id
report_id
user_id
created_at

Existe una restricción:

UNIQUE(report_id, user_id)

para impedir corroboraciones duplicadas.

resources

Contiene recursos que pueden ser publicados en el mapa:

hospitales;
bomberos;
refugios;
puntos de ayuda;
otros recursos verificados.

Los recursos no deben ser publicados como oficiales sin una fuente verificable.

earthquakes

Contiene información sísmica procedente de la fuente correspondiente.

🧩 Tecnologías
Frontend
HTML5
CSS3
JavaScript
MapLibre GL JS
Progressive Web App (PWA)
Backend
Supabase
PostgreSQL
PostGIS
Supabase Auth
Supabase Storage
PostgreSQL RPC
Row Level Security
Offline
IndexedDB
Service Worker
Cache API
Sync Queue
🚀 Instalación
1. Clonar el repositorio
git clone https://github.com/TU-USUARIO/mapa-vivo-colombia.git

cd mapa-vivo-colombia
2. Configurar Supabase

Crear un proyecto en Supabase.

Configurar:

Authentication
Anonymous Sign-Ins

Crear la base de datos utilizando el SQL incluido en:

supabase/schema.sql
3. Crear Storage

Crear el bucket:

report-images

y aplicar las políticas de Storage incluidas en el esquema.

4. Configurar el frontend

Configurar las variables públicas de Supabase:

const SUPABASE_URL = 'TU_SUPABASE_URL';

const SUPABASE_ANON_KEY = 'TU_SUPABASE_ANON_KEY';
IMPORTANTE

La clave anon puede utilizarse en aplicaciones frontend cuando las tablas están correctamente protegidas mediante RLS.

Nunca publicar:

service_role
secret keys
private keys
credenciales administrativas
🧪 Pruebas recomendadas

Antes de desplegar una nueva versión deben comprobarse como mínimo:

Prueba 1 — Crear reporte
Usuario
↓
Crear reporte
↓
Supabase
↓
reports
Prueba 2 — Dos dispositivos
TELÉFONO A
↓
crear reporte

SUPABASE
↓
reports

TELÉFONO B
↓
consultar mapa
↓
ver reporte
Prueba 3 — Corroboración
TELÉFONO B
↓
corroborar

RPC
↓
report_confirmations
↓
COUNT
↓
reports.verification_count
Prueba 4 — No autocorroboración
Usuario A
↓
su propio reporte
↓
corroborar
↓
❌ rechazado
Prueba 5 — No duplicación
Usuario B
↓
corroborar
↓
corroborar nuevamente
↓
❌ rechazado
Prueba 6 — Offline
Sin Internet
↓
crear reporte
↓
IndexedDB
↓
sync_queue
↓
Internet vuelve
↓
Supabase
Prueba 7 — Seguridad

Un usuario no debe poder:

Modificar reporte de otro usuario
Modificar corroboraciones
Crear recursos oficiales arbitrariamente
Acceder a credenciales privadas
🤝 Contribuciones

Este proyecto está abierto a contribuciones.

Si encuentras un error, tienes una mejora o quieres desarrollar una nueva función:

Haz un fork del repositorio.
Crea una rama:
git checkout -b feature/nueva-funcion
Realiza tus cambios.
Prueba la aplicación.
Haz commit:
git commit -m "Añade nueva función"
Haz push:
git push origin feature/nueva-funcion
Abre un Pull Request.
💡 Áreas donde puedes contribuir

Especialmente se buscan contribuciones relacionadas con:

🗺️ Geografía
PostGIS.
análisis espacial;
mapas de afectación;
clustering;
geocodificación;
visualización territorial.
📱 PWA
rendimiento móvil;
funcionamiento offline;
Service Workers;
sincronización;
optimización para dispositivos de bajos recursos.
🧠 Inteligencia comunitaria
sistemas de corroboración;
detección de duplicados;
análisis de confiabilidad;
priorización de reportes.
🚨 Emergencias
modelos de clasificación de incidentes;
accesibilidad;
diseño para situaciones de estrés;
protocolos de información.
🔐 Seguridad
RLS;
protección contra abuso;
rate limiting;
validación;
privacidad.
🎨 Interfaz
accesibilidad;
UX móvil;
visualización de información crítica;
diseño para personas con discapacidad.
📊 Datos
integración de fuentes oficiales;
validación de recursos;
normalización de datos;
nuevas fuentes públicas.
⚠️ Principios del proyecto
1. No inventar información

Nunca introducir datos ficticios como si fueran reales.

Especialmente:

hospitales;
refugios;
vías;
terremotos;
personas afectadas;
recursos de emergencia.
2. Diferenciar fuente oficial y reporte comunitario

La interfaz debe distinguir claramente entre:

FUENTE OFICIAL

y:

REPORTE COMUNITARIO

La corroboración comunitaria tampoco convierte automáticamente un reporte en información oficial.

3. Privacidad

No publicar innecesariamente:

nombres;
teléfonos;
direcciones particulares;
información médica identificable;
fotografías que expongan información personal sensible.
4. Seguridad antes que funcionalidades

Una función nueva no debe incorporarse si puede:

facilitar desinformación;
exponer datos personales;
comprometer la infraestructura;
generar información falsa durante una emergencia.
🛣️ Roadmap
Fase 1 — MVP
 Mapa interactivo
 Reportes comunitarios
 Geolocalización
 Supabase
 PostgreSQL
 PostGIS
 RLS
 Corroboración
 PWA
 IndexedDB
 Funcionamiento offline
 Pruebas públicas
Fase 2
 Supabase Realtime
 Optimización de sincronización
 Mejor geocodificación
 Filtros territoriales
 Clustering de incidentes
 Mejoras de accesibilidad
 Moderación comunitaria
Fase 3
 Integración con más fuentes oficiales
 Análisis espacial avanzado
 Alertas geográficas
 Herramientas para organizaciones
 Panel de análisis de emergencias
🌐 Código abierto

Mapa Vivo Colombia se desarrolla como un proyecto abierto para que desarrolladores, investigadores, artistas, organizaciones, comunidades y ciudadanos puedan contribuir a su evolución.

El código abierto permite:

auditar el funcionamiento;
detectar errores;
proponer mejoras;
adaptar la herramienta;
desarrollar nuevas funcionalidades;
crear soluciones complementarias.

Las contribuciones deben priorizar la seguridad, privacidad, accesibilidad y utilidad comunitaria.

📜 Licencia

Este proyecto se distribuye bajo la licencia:

MIT License

Consulta el archivo:

LICENSE

para conocer los términos completos.

👤 Autor / Proyecto

Mapa Vivo Colombia

Proyecto comunitario de tecnología abierta para información colaborativa ante emergencias.

🤝 ¿Quieres contribuir?

Si eres desarrollador, diseñador, investigador, especialista en emergencias, geógrafo, artista, comunicador o simplemente tienes una propuesta útil:

abre un Issue o Pull Request.

La plataforma puede mejorar con la participación de la comunidad.

⚠️ Aviso final

Mapa Vivo Colombia es una herramienta tecnológica comunitaria.

No garantiza la exactitud, disponibilidad o actualidad de los reportes realizados por usuarios.

No reemplaza:

organismos de socorro;
autoridades locales;
sistemas oficiales de alerta;
servicios médicos;
líneas oficiales de emergencia.

En una situación de emergencia, utiliza siempre los canales oficiales disponibles.
