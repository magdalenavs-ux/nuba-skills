---
name: "nuba-cotizar-hoteles"
description: "Cotizar hoteles para clientes NUBA en Expedia TAAP y Sabre Red Web (con código NEM) y armar tabla comparativa. Usar cuando Maguis pida cotizar un hotel o comparar TAAP vs Sabre."
---

# Cotizar hoteles NUBA (TAAP + Sabre)

## Reglas de precio
- TAAP y Sabre ya traen comisión incluida: NO sumar comisión NUBA.
- Solo los precios netos de DMC llevan comisión NUBA (12–15%).
- Si no se indica ocupación, cotizar 2 adultos / 1 habitación y decirlo.

## Navegador
- Usar el navegador integrado de la app (built-in browser). Pedir acceso al sitio con scope "site" la primera vez.
- Nunca escribir contraseñas: si la sesión no está iniciada, pedir a Maguis que inicie sesión. Si el panel está oculto, indicarle el ícono del globo arriba a la derecha (o Cmd+Shift+B).

## Expedia TAAP (https://www.expediataap.com)
1. Búsqueda por URL: `https://www.expediataap.com/Hotel-Search?destination=<Hotel, Ciudad, País>&startDate=AAAA-MM-DD&endDate=AAAA-MM-DD&rooms=1&adults=2`
2. Si el hotel no aparece, usar el filtro por nombre: añadir `&hotelName=<palabra clave>` (p. ej. "Relais" encontró Riad Fès – Relais & Châteaux).
3. Sacar la URL de la ficha desde los links `...Hotel-Information` de la página y abrir con `?chkin=...&chkout=...&rm1=a2`.
4. Leer "Choose your room" con get_page_text: precio por noche, total "includes taxes & fees", opciones Non-Refundable / Fully refundable (+ $X por noche), fecha límite de pago ("Pay Expedia now or by ..."), extras (desayuno).
5. Para el total exacto reembolsable: hacer clic en el radio "Fully refundable before ..." y releer el total.
6. Anotar cargos en destino de "Fees" (impuesto municipal, desayuno, traslados).

## Sabre Red Web (https://srw.sabre.com)
- Formato de búsqueda (siempre con NEM): `HOT{ciudad}¥NEM/{DDMMM}-{DDMMM}{personas}/N-{hotel}`
  - Ej.: `HOTRAK¥NEM/17FEB-20FEB2/N-LA MAMOUNIA`, `HOTFEZ¥NEM/20FEB-23FEB2/N-RIAD FES`
  - `/NAME-` NO funciona; usar `/N-`.
- Antes de escribir un comando: clic en la línea de comandos, Cmd+A y Backspace (puede haber texto previo).
- Si un hotel tiene tarifa negociada aparece la etiqueta NEM y "Negotiated Rate: Yes" al expandirlo.
- Ver tarifas: clic en el nombre del hotel → botón "View Rates". Expandir cada tarifa para leer "Cancellation policy", "Guarantee/Deposit" e impuestos.
- Las tarifas salen en moneda local (p. ej. MAD). Convertir con el tipo de cambio de Sabre: `DC‡MAD1000/USD`.
- Ojo: en tarifas prepago el depósito suele cobrarse el día de la reserva.
- Si no hay resultados con nombre, probar sin `/N-` para ver qué hoteles tiene Sabre en esa ciudad.

## Entregable
Tabla comparativa por hotel: habitación, total USD (impuestos incluidos), qué incluye (desayuno), cancelación gratuita hasta, forma/fecha de pago, y "Mejor opción". Señalar: cargos que se pagan en el hotel, disponibilidad nula, y datos por confirmar. Hoteles que no estén en TAAP ni Sabre → sugerir DMC o venta directa (neto + comisión NUBA).