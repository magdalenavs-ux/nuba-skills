---
name: "nuba-core-cobro"
description: "Crear un cobro en NUBA Core (core.nuba.com) asociado a un expediente y reenviar la solicitud de cobro al cliente. Usar cuando Maguis pida registrar un cobro, anticipo o pago de cliente en NUBA Core."
---

# Cobro en NUBA Core

## Acceso
- Navegador integrado → https://core.nuba.com. Si pide login, usar el botón "Microsoft" (SSO, entra con la sesión de Maguis); nunca escribir usuario/contraseña.
- Si el expediente aún no existe, crearlo primero con la skill `nuba-core-expediente`.
- La vista estrecha oculta columnas: emular viewport 1440x900 mientras se trabaja y volver a "desktop" al terminar.

## Pasos
1. Búsqueda de expedientes → escribir el número en "Expediente" → BUSCAR.
2. Abrir el expediente con el ícono de lápiz al final de la fila.
3. Verificar jurisdicción (Agencia MEX_..., ID cliente CACMX...). Si no coincide, detenerse y reportar.
4. Pestaña "Cobros" (abajo, junto a Pasajeros / Servicios / Pagos): revisar que no exista ya un cobro igual (evitar duplicados). Si el expediente ya está abierto, no hace falta buscarlo.
5. Botón "+" → "Nuevo cobro".
6. Campos:
   - Administrar expediente en: Nuba Exp de México (MX) / Nuba USA INC.
   - Destino del viaje: NACIONAL / INTERNACIONAL.
   - Requiere factura: si es "Sí", exige adjuntar constancia de situación fiscal (≤ 3 meses). Preguntar a Maguis; si no la tiene, "No" y facturar después (solo en el mes en que se recibe el pago).
   - Método de pago: Tarjeta de crédito / Transferencia MXN / EUR / USD. La divisa se ajusta sola.
   - Importe: el campo tiene máscara. Clic, End, Backspace ×15 hasta vaciar; luego teclear dígito por dígito (key "8", "4"…) verificando el valor después de cada uno. Debe leerse p. ej. 845,750.00.
   - Cliente: hay homónimos. Escribir parte del nombre, esperar las opciones y hacer clic físico en la opción (no seleccionarla por JS). Confirmar que "ID Cliente" coincide con el del expediente. Al elegirlo se llena solo "Nombre y Apellido/Empresa" en los datos de factura.
   - Fecha de vencimiento: viene con la fecha de hoy.
   - Concepto (obligatorio) y Email (obligatorio en datos de factura; no se autocompleta).
7. Orden recomendado: primero Requiere factura (cambiarla vacía Cliente), luego Cliente (vacía Email), luego Email, Concepto e Importe.
8. Revisar que ningún campo esté marcado como inválido y mostrar resumen a Maguis antes de GUARDAR si hay algún dato que ella no haya dado.
9. Después de guardar ("Cobro guardado correctamente"), en la pestaña Cobros reportar: ID, ID externo, RCC, importe, método, concepto, estado. Regresar el viewport a "desktop".

## Enviar la solicitud de cobro al cliente
La solicitud de cobro **siempre se manda al cliente**.
10. Preguntar a Maguis el email del cliente. No usar el email capturado en el cobro sin confirmarlo con ella.
11. En Outlook (outlook.cloud.microsoft, navegador integrado), buscar el correo de **Notificaciones B.Central** (notificacionesbc@nuba.com) con el asunto "Proforma factura <ID externo> for …". El ID externo es el BO_FV… del cobro. Llega unos minutos después de guardar.
12. Abrirlo → **Reenviar**. Verificar que trae el adjunto "SolicitudCobro<ID externo>.pdf".
13. En "Para", escribir el email del cliente y confirmar que el destinatario se resolvió bien.
14. Clic en **Enviar**. El ref del botón puede quedar desfasado; si no se envía, hacer clic en la posición visible del botón según una captura nueva.
15. Confirmar en el hilo que el reenvío aparece enviado y reportar a Maguis la hora y el destinatario.