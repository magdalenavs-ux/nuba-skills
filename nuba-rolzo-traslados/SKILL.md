---
name: "nuba-rolzo-traslados"
description: "Cotizar y reservar traslados, chofer por horas y VIP Meet & Greet en ROLZO Business para clientes NUBA en EE. UU., Europa y Asia. Usar cuando Maguis pida cotizar o reservar transporte terrestre fuera de México."
---

# Traslados con ROLZO (NUBA)

ROLZO es el proveedor de NUBA para transporte en **Estados Unidos, Europa y Asia**. La cuenta de Maguis tiene estatus **ROLZO First** por la alianza con NUBA.

## Acceso
- Navegador integrado de la app: https://business.rolzo.com (sitio público: https://www.rolzo.com). Si pide permiso para el sitio, solicitarlo con alcance "site" para `rolzo.com` y `business.rolzo.com`.
- La sesión suele estar iniciada ("Hi, Magdalena"). Si no, pedir a Maguis que entre. Nunca escribir contraseñas.
- La vista angosta esconde campos: emular `resize_window` 1440x900 mientras se trabaja y regresar a `desktop` al terminar.

## Cotizar un traslado
1. Ir a https://business.rolzo.com/booking/main/new-booking → **Transfer** (también hay **By the hour** y **VIP Meet & Greet**).
2. **Fecha (PICK UP DATE):** clic en el campo y elegir el día con JS para no fallar por coordenadas:
   - Cambiar de mes con la flecha del calendario si hace falta.
   - `[...document.querySelectorAll('.react-datepicker__day--0DD')].filter(e=>!e.className.includes('outside-month'))[0].click()` (DD = día con 2 dígitos).
   - Verificar el valor del campo (ej. "Sun, 27th Dec 2026").
3. **Pick-up location:** escribir (ej. "Charles de Gaulle") y hacer clic en la sugerencia correcta (ej. "Aéroport de Paris-Charles de Gaulle (CDG)").
4. **Pick-up time:** poner la hora de llegada del vuelo si se conoce; si no, dejar la que trae y avisar que se cotizó con esa hora.
5. **Drop-off location:** escribir el hotel o dirección y elegir la sugerencia (ej. "Hôtel Lutetia - Paris 6, Boulevard Raspail").
6. **Search** → leer la lista de vehículos con `innerText` de `main`: categoría, modelo, pasajeros, maletas y precio.

## Presentar la cotización
- Tabla: vehículo, capacidad (pasajeros / maletas), precio en la moneda que da ROLZO (EUR en Europa, USD en EE. UU.). Incluir distancia y tiempo.
- Recomendar el vehículo que alcance para pasajeros **y** maletas. Referencia en París:
  - E-Class (Business) 2/2 · S-Class (First) 2/2 · V-Class (Minivan) 6/6 · Sprinter (Van) 12/12.
  - Para 4 personas con 6 maletas: V-Class. Si quieren sedán, son 2 autos.
- Los precios de ROLZO son netos para la agencia salvo que Maguis indique otra cosa: preguntar si aplica comisión NUBA antes de pasar precio al cliente.

## Reservar
- Antes de dar **Book**, pedir: número de vuelo y hora de llegada (para seguimiento del vuelo), nombre del pasajero principal, celular de contacto, número de pasajeros y maletas, y notas (cartel con nombre, silla de bebé, etc.).
- Confirmar con Maguis vehículo y total antes de reservar.
- Si pide forma de pago con tarjeta: no capturarla; que Maguis la ingrese o use 1Password si ella lo aprueba.
- Al terminar, reportar el número de reserva de ROLZO y ofrecer registrar el servicio en el expediente de NUBA Core (tipo Traslado, proveedor ROLZO).

## Consultas de reservas existentes
- My bookings: https://business.rolzo.com/booking/main/my-bookings
- Detalle: `/booking/my-bookings/details/<id>`. El chat de soporte 24/7 está en la misma cuenta.