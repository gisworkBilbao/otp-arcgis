# Ruta de Transporte Público en Mapa

Este proyecto es una aplicación web que utiliza ArcGIS y una API de transporte público para mostrar las rutas de transporte público en un mapa interactivo. Los usuarios pueden ingresar una dirección de inicio y destino, ver las rutas posibles y visualizar el itinerario en el mapa, incluyendo las paradas de buses y el modo de transporte.

## Descripción

La aplicación permite al usuario:
- Ingresar una dirección de origen y destino.
- Buscar rutas de transporte público utilizando un servicio de geocodificación.
- Ver opciones de rutas y su respectivo itinerario.
- Mostrar las paradas de buses en el mapa.
- Ver los iconos de origen y destino en el mapa.

## Características

- **Búsqueda de direcciones**: Los usuarios pueden ingresar cualquier dirección y la aplicación mostrará posibles resultados, con la opción de seleccionar el destino deseado.
- **Opciones de ruta**: Después de obtener las ubicaciones de inicio y destino, la aplicación muestra diferentes opciones de ruta de transporte público.
- **Dibujo de rutas en el mapa**: Se dibujan las rutas en el mapa con diferentes colores y estilos según el modo de transporte (caminar, bus, etc.).
- **Iconos personalizados**: Los iconos para el origen y destino se muestran en el mapa, además de los íconos de las paradas de buses.

## Requisitos

Para ejecutar este proyecto, necesitas tener lo siguiente:

- Un servidor web (puede ser local o remoto).
- Acceso a la API de ArcGIS para renderizar el mapa y obtener geolocalización.
- Una fuente de datos de rutas de transporte público que se pueda consultar mediante GraphQL (la estructura de la API debe ser adecuada para este propósito).
- Archivos de iconos para el origen y destino (`origin-icon.png` y `destination-icon.png`).

## Instalación


# Implementación y Uso de OpenTripPlanner (OTP)

Este repositorio proporciona los pasos para instalar, configurar y utilizar OpenTripPlanner (OTP), un sistema de planificación de viajes multimodal de código abierto. OTP soporta diversos modos de transporte como público, bicicleta, caminata y combinaciones de estos. 

## Índice

1. [Introducción](#introducción)
2. [Instalación de OpenTripPlanner](#instalación-de-opentripplanner)
3. [Configuración de Datos](#configuración-de-datos)
4. [Construcción del Gráfico de Tránsito](#construcción-del-gráfico-de-tránsito)
5. [Inicio y Uso del Servidor OTP](#inicio-y-uso-del-servidor-otp)
6. [Optimización y Manejo Avanzado](#optimización-y-manejo-avanzado)
7. [Guardado y Carga de Gráficos](#guardado-y-carga-de-gráficos)
8. [Desarrollos Avanzados](#desarrollos-avanzados)
9. [Resolución de Problemas](#resolución-de-problemas)
10. [Limitaciones y Consideraciones](#limitaciones-y-consideraciones)

## Introducción

### ¿Qué es OpenTripPlanner?
OpenTripPlanner (OTP) es un sistema de planificación de viajes multimodal de código abierto que permite a los usuarios planificar rutas combinando diferentes modos de transporte, incluyendo transporte público, bicicleta y caminata.

**Versión actual**: OTP 2.6.0  
**Requisitos de Java**: Java 21 o posterior.

### Requisitos previos

#### Hardware
- **Mínimo**: 1GB de RAM.
- **Recomendado**: 2GB o más para conjuntos de datos grandes.

#### Software
- **Java JDK 21 o posterior**  
  [Descargar Java 21](https://download.oracle.com/java/21/archive/jdk-21.0.4_windows-x64_bin.exe)

Para verificar la instalación de Java, ejecuta el siguiente comando en la terminal:

```bash
java -version
```

Si no se muestra la versión correctamente, reinicia el equipo.

## Instalación de OpenTripPlanner

1. **Descarga del archivo JAR de OTP**  
   Descarga desde Maven Central:  
   [OTP 2.6.0](https://repo1.maven.org/maven2/org/opentripplanner/otp/2.6.0/otp-2.6.0-shaded.jar)

2. **Compilación desde código fuente (opcional)**  
   Si prefieres compilar desde el código fuente, clona el repositorio y sigue las instrucciones de compilación.

   ```bash
   git clone https://github.com/opentripplanner/OpenTripPlanner.git
   cd OpenTripPlanner
   mvn clean package
   ```

## Configuración de Datos

### Datos Requeridos
OTP requiere dos tipos de datos:

- **GTFS**: Horarios y paradas del transporte público.
- **OSM**: Mapa detallado de redes viales.

### Obtención de Datos

#### GTFS
- **Fuentes recomendadas**:
  - [Transitland](https://www.transit.land/feeds/f-ezjm-empresamunicipaldetransportes)
  - [TransitFeeds](https://transitfeeds.com/l/166-spain)
  - [Bizkaibus GTFS](https://baliabideak.bizkaia.eus/Bizkaibus/GTFS/)

#### OSM
- **Fuentes recomendadas**:
  - [Geofabrik](https://download.geofabrik.de/europe/spain/pais-vasco.html)
  - [Protomaps](https://app.protomaps.com/)

Para instalar **osmconvert**, consulta la [guía de instalación](https://disk.yandex.ru/d/Vnwc4kut3LCBFm).

### Ejemplo de Descarga y Preparación de Datos

```bash
# Crear directorio para datos
mkdir /home/username/otp-data/routers/default

# Descargar y descomprimir GTFS
Invoke-WebRequest "https://baliabideak.bizkaia.eus/Bizkaibus/GTFS/Bizkaibus.zip" -O bizkaibus.gtfs.zip

# Descargar y procesar OSM
Invoke-WebRequest "https://download.geofabrik.de/europe/spain/pais-vasco-latest.osm.pbf"
osmconvert pais-vasco-latest.osm.pbf -b=43.29504,-2.989254,43.26236,-2.934894 --complete-ways -o=pais-vasco.pbf
mv pais-vasco.pbf C:\otp-data
outers\default
```

## Construcción del Gráfico de Tránsito

1. **Construir el gráfico completo** (transporte y red vial):

   ```bash
   java -Xmx2G -jar otp-2.6.0-shaded.jar --build /ruta/a/datos --serve
   ```

2. **Construir solo la red vial**:

   ```bash
   java -Xmx2G -jar otp-2.6.0-shaded.jar --buildStreet .
   ```

3. **Cargar y servir el gráfico**:

   ```bash
   java -Xmx2G -jar otp-2.6.0-shaded.jar --load --serve --port 8080 .
   ```

## Inicio y Uso del Servidor OTP

1. **Iniciar el servidor OTP**:

   ```bash
   java -Xmx2G -jar otp-2.6.0-shaded.jar --serve --port 8080
   ```

2. **Visualización**:
   Accede a la interfaz web a través de [http://localhost:8080](http://localhost:8080).

3. **Interacción con la API**:
   Usa la interfaz JavaScript para interactuar con la instancia local.

## Optimización y Manejo Avanzado

### Optimización de Memoria

- Usa el parámetro `-Xmx` para ajustar la cantidad de memoria que Java puede usar. Ejemplo para 4GB:

  ```bash
  java -Xmx4G -jar otp-2.6.0-shaded.jar --serve
  ```

- **Herramientas de monitoreo**:
  - **VisualVM** para análisis de memoria.
  - **VisualGC** plugin para ver la recolección de basura de Java.

### Mejores Prácticas para Grandes Volúmenes de Datos

- Divide el proceso de construcción de gráficos y ejecución del servidor en dos fases.
- Utiliza máquinas de alto rendimiento o instancias en la nube.

## Guardado y Carga de Gráficos

1. **Guardar el gráfico para reutilización**:

   ```bash
   java -Xmx2G -jar otp-2.6.0-shaded.jar --build --save /ruta/a/otp/
   ```

2. **Cargar gráficos guardados**:

   ```bash
   java -Xmx2G -jar otp-2.6.0-shaded.jar --load /ruta/a/otp/
   ```

## Desarrollos Avanzados

### Clonación y Personalización

1. Clona el repositorio y compila OTP desde el código fuente.

   ```bash
   git clone https://github.com/opentripplanner/OpenTripPlanner.git
   cd OpenTripPlanner
   mvn clean package
   ```

2. **Personalización**: Puedes modificar los algoritmos de enrutamiento y agregar nuevos modos de transporte.

### Integración con Otras APIs

OTP ofrece una API REST para integrarse con otros sistemas. Ejemplo de solicitud:

```bash
GET /otp/routers/default/plan?fromPlace=45.5,-122.6&toPlace=45.5,-122.7&time=1:02pm&date=11-14-2023&mode=TRANSIT,WALK
```

## Resolución de Problemas

### Actualización de OTP

1. Descarga la nueva versión de OTP y reemplaza el archivo `otp-shaded.jar`.
2. Actualiza las dependencias si estás trabajando desde el código fuente:

   ```bash
   mvn clean install
   ```

### Configuración para Múltiples Agencias de Transporte

Coloca los archivos GTFS en su respectivo directorio y ajusta la configuración en `router-config.json`.

### ¿Puerto 8080 en uso?

Puedes cambiar el puerto de OTP con el siguiente comando:

```bash
java -Xmx2G -jar otp-shaded.jar --serve --port 8081
```

## Limitaciones y Consideraciones

- **Cobertura geográfica**: OTP puede no tener cobertura completa en algunas áreas.
- **Precisión de los datos**: La calidad de las rutas depende de los datos de GTFS y OSM.
- **Preferencias del usuario**: Algunas preferencias específicas pueden no ser consideradas.


**Clonar el repositorio**:
   ```bash
   git clone https://github.com/tu-usuario/nombre-del-repositorio.git
   cd nombre-del-repositorio
