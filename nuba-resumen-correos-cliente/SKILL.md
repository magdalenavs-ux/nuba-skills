---
name: "nuba-resumen-correos-cliente"
description: "Buscar en Outlook todos los correos de un cliente o destino NUBA y resumir el estado del viaje: datos, vuelos, pagos, actividades y pendientes con proveedores."
---

# Resumen de correos de un cliente o viaje NUBA

Usar cuando Maguis pida "checa todos los correos de [cliente/destino]" o quiera saber en qué va un viaje.

## Pasos
1. Cargar la skill `built-in-browser`. Abrir `https://outlook.office.com/mail/` y pedir acceso (scope "site") a `outlook.office.com` y `outlook.cloud.microsoft` si hace falta.
2. Hacer clic en el cuadro "Buscar", escribir el destino y el apellido (por ejemplo "Marruecos Slim") y presionar Enter.
3. Leer la lista de resultados con `get_page_text` e identificar los hilos principales:
   - el hilo del DMC u hotel
   - Nuba Core (justificantes de pago)
   - Connect/Egresos
   - las propuestas viejas de otros proveedores
4. Abrir el hilo principal (con `find` y clic por ref) y leerlo completo con `get_page_text`, con max_chars de unos 30000. El hilo viene del más reciente al más antiguo.
5. Si hay más hilos relevantes, abrirlos también. Ojo: en el panel angosto la lista se oculta al abrir un correo, así que hay que regresar con `navigate` "back".

## Formato de la respuesta (en español, conciso)
- **Estado actual:**
  - pasajeros y fechas
  - vuelos, con número, horario y localizador
  - pagos realizados (montos, fechas y quién confirmó)
  - actividades confirmadas
- **Pendientes:** lo que se pidió y el proveedor no ha respondido, con la fecha en que se pidió y cuántos días lleva sin respuesta. Señalar lo crítico, por ejemplo un traslado que se cruza con el horario de un vuelo.
- **Otros hilos:** propuestas descartadas u otros proveedores, en una línea.
- Terminar ofreciendo redactar un correo de seguimiento al proveedor. Nunca enviarlo sin la confirmación de Maguis.

## Notas
- Distinguir entre el asunto del hilo y las fechas reales. Por ejemplo, un asunto "DEC2025/26" puede ser de un viaje de dic 2026 – ene 2027; las fechas se toman de los vuelos.
- No inventar datos que no estén en los correos. Si algo no aparece, decirlo.