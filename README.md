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

1. **Clonar el repositorio**:
   ```bash
   git clone https://github.com/tu-usuario/nombre-del-repositorio.git
   cd nombre-del-repositorio
