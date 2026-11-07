# Mapa de expansión urbana de Quito

Sitio estático. No necesita base de datos ni backend: se sube la carpeta tal cual a cualquier servidor web (Apache, Nginx, IIS, Netlify, GitHub Pages, Cloudflare Pages, un bucket S3, etc.).

## Contenido

```
mapa-expansion-quito/
├── index.html              Página del mapa (HTML, CSS y lógica)
├── assets/leaflet/         Leaflet 1.9.4 (librería de mapas, licencia BSD-2)
├── data/mapa-data.js       Todos los datos del mapa (~1,8 MB)
├── .htaccess               Compresión y caché para Apache (opcional)
└── README.md
```

## Publicar

1. Sube la carpeta completa al directorio público del servidor (por ejemplo `/var/www/html/mapa-quito/`).
2. Abre `https://tu-dominio/mapa-quito/`.

También funciona sin servidor: basta con abrir `index.html` con doble clic.

### Recomendado: compresión

`data/mapa-data.js` pesa 1,8 MB y se reduce a unos 450 KB con gzip.

- **Apache:** el `.htaccess` incluido ya activa la compresión y la caché (requiere `mod_deflate` y `mod_expires`).
- **Nginx:** agrega esto al bloque `server`:

```nginx
gzip on;
gzip_types application/javascript text/css text/html;
location ~* \.(js|css|png)$ { expires 7d; }
```

## Dependencias externas

La única es Google Fonts (tipografías Archivo, Public Sans e IBM Plex Mono). Si el servidor no tiene internet o prefieres no usar Google, borra las dos líneas `<link ... fonts.googleapis.com ...>` de `index.html`. El mapa seguirá funcionando con las fuentes del sistema.

El mapa no usa teselas de fondo (OpenStreetMap, Google, etc.). Todo lo que se dibuja está en `data/mapa-data.js`.

## Actualizar los datos

`data/mapa-data.js` define `window.MAPA_DATA` con estas claves:

| Clave | Contenido |
|---|---|
| `parr` | 65 parroquias (límites, población 2010 y 2022, hectáreas de expansión y de suelo no consolidado) |
| `suelo`, `planes`, `cent` | Subclasificación del suelo, planes urbanísticos y centralidades del PUGS 2024 |
| `metro`, `est`, `aero`, `vias`, `vproy` | Metro, aeropuerto, vías principales y vías proyectadas |
| `lahar` | Zona de lahares del Cotopaxi |
| `grid`, `pollu`, `vtraf`, `indz`, `lic`, `riop`, `ext` | Capa de contaminación: cuadros de 300 m, índice por parroquia, vías de alto tráfico, zonas industriales, industrias con licencia, ríos contaminados y canteras |
| `lots` | Terrenos en venta de Plusvalía (precio, área, coordenadas, enlace) |

Los precios agregados por parroquia (`PRICES`) y los riesgos (`RISK`) están dentro de `index.html`.

## Fuentes y fecha de corte

- Municipio de Quito, GeoQuito: PUGS 2024 (Ordenanza PMDOT-PUGS 003-2024), red vial, cuencas hidrográficas, licencias industriales. Descargado el 7 de noviembre de 2026.
- INEC, Censos 2010 y 2022, según el Instituto de Investigaciones de la Ciudad (Boletín 4, 2024).
- Plusvalía.com: anuncios de terrenos al 7 de noviembre de 2026. Son precios de oferta y caducan rápido. Revisa los términos de uso de Plusvalía antes de publicar el mapa en un sitio abierto.

Es un mapa informativo. No es asesoría financiera ni legal. La zonificación de un lote concreto la confirma su IRM.
