---
name: "nuba-propuesta-pdf"
description: "Crear la propuesta de viaje en PDF para un cliente NUBA con la marca de NUBA, fotos aspiracionales y firma en texto. Usar cuando Maguis pida un PDF o propuesta para un cliente."
---

# Propuesta PDF NUBA

## Formato
- Vertical, tamaño carta (8.5 x 11 in), 1–2 páginas.
- Página 1: foto grande arriba a todo lo ancho con logo NUBA blanco y título del destino; destinatario y datos de la agencia; resumen (duración, hoteles, total); itinerario día por día.
- Página 2: franja de 3 fotos; Inversión (tabla de hoteles con total); Incluye / No incluye; Forma de pago; Cancelación; Cambios y documentación; Información importante; firma.
- Todo dato que no esté en la conversación o en la ficha del cliente → "[por confirmar]". No inventar servicios ni condiciones.

## Marca (carpeta "NUBA RECURSOS" en el Escritorio)
- Colores: café #79411C, crema #EFEBE6, beige #D6C0A9, beige claro #E4DCD3. No usar otros (nada de rosa).
- Tipografías: HW Cigars Light (títulos, mayúsculas) y Neue Haas Grotesk Display 45/55/65 (texto). Archivos en NUEVAS TIPOGRAFIAS; añadir word-spacing ~0.14em al texto.
- Logo: LOGOTIPOS/Logo Nuba Genérico_Marrón; para versión blanca, recolorear desde la marrón usando la luminancia como alfa.

## Fotos
- Estilo del brandbook: aspiracionales, paisajes amplios, tonos cálidos, arquitectura con textura. Nada de fotos de catálogo tipo alberca.
- NO usar fotos con derechos (National Geographic, etc.). Usar Unsplash (licencia libre comercial).
- Ruta que funciona: en el navegador integrado abrir unsplash.com, sacar IDs `photo-...` del HTML de `/es/s/fotos/<búsqueda>?license=free`, revisar con una hoja de miniaturas, y descargar a ~/Downloads de una en una (las descargas múltiples se bloquean; esperar unos segundos entre cada una). Pedir acceso a ~/Downloads, luego stage de los archivos.
- Oscurecido para legibilidad: integrarlo en la imagen con PIL (no usar capas CSS semitransparentes: en Preview salen rosas). Verificar que el PDF no tenga /SMask ni /Group.

## Firma
- No pegar la imagen de la firma. Reproducirla en texto: "Magdalena De La Viña", "Luxury Travel Advisor · NUBA", magdalena.delavina@nuba.com, +52 (55) 5255 4455; sellos circulares "A NUBA TOP Producer 2026" y "Elite Member NUBA 2026"; banda café "VIRTUOSO Member | Top Producer Hotels & Resorts · Top Producer DMCs · Top Producer Cruise".

## Construcción
- HTML con @font-face locales → Chrome headless `--print-to-pdf --no-pdf-header-footer`.
- Revisar: número de páginas, que nada se encime con el pie, totales sumados correctamente. Créditos de fotos en letra pequeña.