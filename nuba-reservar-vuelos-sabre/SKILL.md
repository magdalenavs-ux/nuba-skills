---
name: "nuba-reservar-vuelos-sabre"
description: "Cotizar y reservar vuelos en Sabre Red Web para clientes NUBA (PNR con DKNEM200, contacto de Maguis, una sola tarifa en pesos y correo de emisión a Mariana Flores). Usar cuando Maguis pida cotizar, reservar o emitir un vuelo en Sabre."
---

# Cotizar y reservar vuelos en Sabre (NUBA)

## Reglas fijas de Maguis (siempre)
- **Customer number:** agregar siempre `DKNEM200` en todo PNR de vuelos. Sabre solo acepta DK de 6, 7 o 10 caracteres.
- **Tarifa: UNA sola cotización guardada y siempre en pesos.**
  - Para cotizar o comparar se usa `WPNC¥MUSD` o `WPNC¥MMXN`, que **no guardan** nada.
  - Para guardar se usa **solo** `WPRQ¥MMXN`, una sola vez por PNR. Nunca `WPRQ¥MUSD`.
  - Antes del ER, revisar con `*PQ` que exista un solo PQ y que diga `RQ¥MMXN`. Si hay otro, borrarlo con `PQD<número>` (ej. `PQD1`).
- **Contacto de Maguis en todo PNR:**
  - Teléfono de agencia: `9525552554455-A` (+52 55 5255 4455).
  - Correo para itinerario: `PE¥MAGDALENA.DELAVINA@NUBA.COM¥`
  - **Contacto para la aerolínea (SSR CTCE/CTCM), por pasajero:**
    `3CTCEA/MAGDALENA.DELAVINA//NUBA.COM-1§3CTCMA/5554323250-1`
    (en CTCE la `@` se escribe `//`; el `-1` es el número de pasajero, repetir con `-2`, `-3`…). Debe aparecer `SSR CTCE … HK1` y `SSR CTCM … HK1`.
- **Recibido de:** `6MAGUIS`
- **Pasaporte (DOCS):** de la imagen del pasaporte se toman nombre, fecha de nacimiento, sexo y vigencia. **Claude no escribe números de pasaporte en Sabre** (regla de seguridad que no se puede saltar). Claude deja a Maguis la línea completa armada en el chat, con el formato de su PCC, para que ella la pegue:
  `3DOCSA/P/MX/<número>/MX/<DDMMMAAAA nacimiento>/<M|F>/<DDMMMAAAA vencimiento>/<APELLIDOS>/<NOMBRES>-<número de pasajero>`
  Ejemplo de Maguis: `3DOCSA/P/MX/1234567890/MX/15AUG1947/M/30SEP2021/DIAZ/JOSE-1`
  (país `MX`, años de 4 dígitos, termina en `-1`, `-2`… según el pasajero). Pedir que verifique el número contra el pasaporte. Revisar que esté vigente durante el viaje y recordar la visa si va a EE. UU.

## Navegador
- Sabre Red Web (https://srw.sabre.com) en el navegador integrado de la app. Nunca escribir contraseñas: si aparece "Sabre Login", pedir a Maguis que entre (Agent ID, contraseña, PCC 6FK2).
- Cada comando: clic en la línea de comandos (`TYPE COMMAND HERE`), `cmd+a` + Backspace, escribir, Enter, esperar y leer la respuesta con JS (`document.body.innerText`, desde el último comando).
- Separador de campos: `§`. Cruz de Lorena: `¥`.
- Antes de empezar, `*A` debe decir `NO DATA`. Si hay algo, preguntar antes de borrarlo.

## Cotizar (sin guardar)
1. Disponibilidad: `1DDMMMORIGDEST` (ej. `123DECMEXSLC`). Para una aerolínea u horario: `120NOVMEXORD0800¥AM`.
2. Vender para cotizar: `01Y3` (1 lugar, clase Y, línea 3) o directo `0AM790Y23DECMEXSLCNN1`.
3. Tarifa más baja: `WPNC¥MMXN` (o `¥MUSD` si Maguis quiere ver dólares). Muestra marca (Básica, Premier Clásica…), condiciones y "LAST DAY TO PURCHASE".
4. Otra clase (ej. Premier): `WC1J` y luego `WPMMXN`.
5. Limpiar al terminar: `XI` (borra segmentos) e `I` (ignora el área de trabajo).
- No funcionan en este PCC: `JR.` (Bargain Finder) ni `¥BR`/`¥BRALL`.

## Reservar (crear PNR)
1. Nombre(s) como en pasaporte: `-APELLIDOS/NOMBRES MR|MRS`.
2. `9525552554455-A§6MAGUIS§DKNEM200`
3. Correo: `PE¥MAGDALENA.DELAVINA@NUBA.COM¥` (va solo, no se combina con otros campos).
4. Segmentos: `0AM686J20NOVMEXORDNN2` (aerolínea, vuelo, clase, fecha, ruta, NN + pasajeros). Verificar `SS`.
5. Contacto para la aerolínea por pasajero: `3CTCEA/MAGDALENA.DELAVINA//NUBA.COM-1§3CTCMA/5554323250-1`.
6. Tarifa: `WPRQ¥MMXN` (una sola vez). Comparar con la cotización. Si no coincide, avisar antes de cerrar.
7. Emisión límite: `7TAW<fecha>/` con el "LAST DAY TO PURCHASE" de la tarifa.
8. Secure Flight sin número (lo captura Claude): `3DOCS/DB/03SEP76/M/APELLIDOS/NOMBRES-1.1` por pasajero. Dar a Maguis la línea `3DOCSA/P/MX/…` para que ella agregue el pasaporte.
9. `*PQ` → confirmar un solo PQ en MXN.
10. `ER` → reportar localizador, segmentos `HK`, total en MXN y fecha límite de emisión. Al terminar, `I` para dejar el área de trabajo vacía.
- Si sale `SIMULTANEOUS CHANGES TO PNR`: `IR` y volver a capturar lo que faltó.
- Para mandar el itinerario por correo desde Sabre: `EMR` (requiere el PE en el PNR).
- No emitir boletos ni capturar tarjetas: eso lo hace Maguis o Mariana.

## Correo de emisión a Mariana Flores (Outlook)
- Cuando Maguis pida mandar a emitir, **enviar directo sin pedir confirmación**.
- Outlook web en el navegador integrado: https://outlook.cloud.microsoft/mail/ → Correo nuevo.
- **Para:** Mariana Flores (mariana.flores@nuba.com). Elegirla de la lista de sugerencias.
- **Asunto:** `EMISIÓN VUELOS - <APELLIDOS/NOMBRES MR|MRS como en el pasaporte> - PNR <localizador>`
- **Cuerpo:** saludo, "Por favor apoyo con la emisión del siguiente boleto", pasajero(s), PNR, vuelo(s) con fecha, ruta, horario y clase/marca, tarifa en MXN, fecha límite de emisión, y el PNR pegado (`*<localizador>`). Firma: "Gracias, Maguis".
- El PNR va pegado en el cuerpo: el navegador integrado no puede adjuntar archivos.
- Después de enviar, confirmar en Elementos enviados.

## Hotel en Sabre (si se reserva en el mismo flujo)
- Requiere dirección de agencia: `W-NUBA EXPEDICIONES DE MEXICO S DE RL DE CV¥GOLDSMITH 60¥CIUDAD DE MEXICO MIGUEL HIDALGO¥11540 MX`.
- Búsqueda con NEM: `HOTCHI¥NEM/20NOV-24NOV2/N-PENINSULA` → nombre del hotel → View Rates → expandir tarifa → Book. Si el botón Book no aparece, repetir la búsqueda `HOT…`.
- La garantía con tarjeta la captura Maguis (o 1Password si ella lo aprueba).

## Al terminar
- Avisar la fecha límite de emisión, lo pendiente (número de pasaporte en DOCS, emisión) y ofrecer registrar el servicio en NUBA Core.
- Si Maguis pide salir de Sabre: revisar `*A` = NO DATA y luego `SO*`.