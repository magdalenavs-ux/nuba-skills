---
name: "nuba-core-expediente"
description: "Crear un expediente nuevo en NUBA Core (cabecera, cliente, pasajero titular) y programar servicios/pagos a hoteles u operadores. Usar cuando Maguis pida abrir un expediente o registrar un servicio o pago a proveedor."
---

# Expedientes y servicios en NUBA Core

## Orden de trabajo
1. Crear o localizar el expediente.
2. Registrar el cobro al cliente con la skill `nuba-core-cobro`.
3. Programar los pagos a operadores y hoteles.
4. Registrar las comisiones.

Pedir a Maguis los datos que falten, agrupados en forma de formulario. NUBA Core es producción: si un dato es de prueba, marcarlo como "PRUEBA" o "NO PAGAR" y avisar antes de guardar.

## Acceso
- Abrir https://core.nuba.com en el navegador integrado y entrar con el botón **Microsoft** (SSO). Nunca escribir credenciales.
- La vista angosta oculta columnas y desacomoda los clics. Emular 1440x900 con `resize_window` mientras se trabaja y volver a `desktop` al terminar o antes de pedirle a Maguis que haga clic.
- Las coordenadas de los `ref` quedan viejas después de hacer scroll. Ubicar los campos con JavaScript (`getBoundingClientRect` y la escala del viewport) o con una captura nueva.

## 1. Nuevo expediente
Menú Expedientes → **Nuevo expediente**. Son 3 pasos: Cabecera, Pasajeros y Servicios.

**Cabecera**
- **Nombre viaje:** `LUGARDEVIAJE_NOMBREDECLIENTE_MESYAÑO`, por ejemplo CANCUN_GARCIA_JUNIO2025.
- **Agencia, Travel Advisor y Línea de negocio:** vienen llenos (MEX_MAGDALENA DE LA VIÑA / Independent Contractors).
- **Fechas:** formato DD/MM/AAAA. El campo puede reformatear lo que se escribe, así que hay que verificar con JS que diga la fecha correcta. Al llenar Fecha inicio, Fecha fin suele quedarse con la misma fecha: borrarla con `cmd+a` + Backspace, reescribir y salir con Tab.
- **Nº adultos:** debe coincidir con los pasajeros que se capturen.
- **Valor aproximado:** el campo tiene máscara de moneda. Hay que teclear dígito por dígito con `key` ("5 0 0 0"); con `type` se ve el valor pero el campo queda inválido. Debe leerse 5,000.00. No acepta centavos: "6 3 0 1 6 2" queda como 630,162.00, así que se redondea al entero (6,302). Para borrarlo, `cmd+a` no funciona: usar End + Backspace repetido y Delete repetido.
- **Divisa:** se elige de la lista desplegable (Peso mexicano, Dólar estadounidense o Euro). Si solo se escribe, se borra.
- **Cliente:** escribir parte del nombre, esperar las opciones y **hacer clic físico** en la opción. Seleccionarla por JS no la amarra y el paso no avanza sin dar error. Confirmar que aparezca "ID Cliente: CACMX…".
  - Los `ref` de las opciones caducan rápido: leer las opciones con JS (`mat-option` + `getBoundingClientRect`) y hacer clic por coordenadas convertidas a la escala de la captura (ancho de captura / 1440).
  - Si hay dos clientes con el mismo nombre (ej. uno con el correo del cliente y otro con el de Maguis), preguntar a Maguis cuál usar. Para Maximiliano Romano Baez usó el registro con su correo de NUBA (CACMX4159).
- **Cliente nuevo:** usar el botón "+". Campos obligatorios: Nombre, Oficina NUBA (Nuba), País e Idioma (ES). Si el sistema avisa de clientes que coinciden, revisar las coincidencias y preguntar a Maguis antes de crear un duplicado.
- Si el botón **Siguiente** no avanza, revisar primero que el Cliente siga seleccionado, porque al regresar a la Cabecera se borra.

**Pasajeros**
- El titular se agrega solo, con el nombre del cliente. **Basta con 1 pasajero**, el titular. No agregar acompañantes.

**Servicios**
- Se puede guardar sin servicios. Dar **Guardar** y reportar el número de expediente (por ejemplo 008803NC2026).
- El número de expediente **no** es el DK de Sabre: en los PNR de Sabre el customer number es siempre `DKNEM200`.

## 2. Servicio y pago a proveedor
Con el expediente abierto, ir a la pestaña **Servicios / Pagos** → "+" → **Nuevo servicio**.

**Datos del servicio**
- **Tipo servicio:** Hotel, Traslado, Actividades, Paquete DMC, Aéreo, etc.
- **Proveedor:** buscar en el catálogo, por ejemplo escribiendo "Cancun", y confirmar con Maguis cuál es.
- **Precio coste:** también lleva máscara, así que se teclea dígito por dígito.
- **Divisa coste:** es un autocompletado con muchas monedas. Escribir "Dólar estado" y elegir la opción por su `ref`. Con flechas puede quedar otra moneda (salió Dirham), así que hay que verificar.
- **Tipo de pago:** Prepago neto, que viene por defecto.
- **Datos del servicio:** Booking reference, Destino, Hotel, Tipo de habitación, fechas y noches. Las fechas vienen del expediente.

**Nuevo pago**
- **Tipo:** Pago, Reembolso o Factura final.
- **Importe:** con máscara, igual que los demás montos.
- **Divisa pago, Fecha de vencimiento y Concepto:** obligatorios.
- **Adjuntar facturas:** es obligatorio. Abre el selector de archivos del sistema, que no se puede manejar desde el navegador integrado, y la app de Claude no admite control de pantalla. Pedirle a Maguis que haga clic en "Adjuntar facturas" y elija el archivo; se le puede marcar el botón con un recuadro. Si el archivo viene de su computadora, revisarlo antes y avisar si no corresponde al proveedor o al monto.
- Luego dar **Agregar pago**, revisar el resumen con Maguis y **Guardar**.
- Avisar que un pago a un proveedor real puede entrar a la cola de Egresos.

## Notas
- Si el panel del navegador se cierra, se pierde lo capturado y no guardado.
- Al terminar, reportar el número de expediente y lo que quedó registrado, y regresar el viewport a `desktop`.