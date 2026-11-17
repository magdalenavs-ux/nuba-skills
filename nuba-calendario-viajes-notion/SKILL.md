---
name: "nuba-calendario-viajes-notion"
description: "Revisar los correos de Outlook del último mes, identificar los viajes de clientes NUBA y organizarlos en un calendario de Notion con proyectos y pendientes por viaje."
---

# Calendario de viajes NUBA en Notion (desde Outlook)

Usar cuando Maguis pida revisar su correo y organizar los viajes de clientes en Notion, o actualizar el calendario que ya existe.

## 1. Abrir Outlook en el navegador integrado
- Cargar la skill `built-in-browser` y abrir `https://outlook.office.com/mail/`, que redirige a `outlook.cloud.microsoft`.
- Pedir acceso con `request_access` (scope "site") a `outlook.office.com` y a `outlook.cloud.microsoft`. La sesión de Maguis ya está iniciada.

## 2. Recorrer la bandeja del periodo (por defecto, el último mes)
- La lista de correos es virtual: solo están en el DOM las filas visibles.
- Con `javascript_tool`:
  - Localizar el contenedor con scroll subiendo desde `[role="option"]` hasta el elemento con `overflowY` distinto de visible.
  - Recorrerlo por pasos (scrollTop += 80% de la altura, con esperas de unos 700 ms).
  - Guardar en un Map el `aria-label` de cada fila y su `data-convid`.
  - Correrlo en segundo plano (sin await) y consultar el avance, porque cada llamada tiene un límite de unos 45 s.
- Descartar el ruido: newsletters, Producto Cruceros, People NUBA, NUBA Training/Forward, invitaciones de proveedores, avisos de aerolíneas y códigos SIXT o ROLZO.

## 3. Leer cada hilo relevante
- No hacer clic en la lista: en el panel angosto, el correo abierto tapa la lista.
- Abrir cada conversación sin recargar la página:
  - `history.pushState({}, '', '/mail/inbox/id/' + encodeURIComponent(convid))`
  - `dispatchEvent(new PopStateEvent('popstate'))`
  - Esperar unos 3 s y leer `innerText` de `[aria-label="Panel de lectura"]`.
- Procesar los hilos en lotes de unos 8 dentro de una cola en segundo plano.
- Hilos clave: los del DMC (ASIBI, Afro Asian, North of Normal, &Beyond, Latitude 1943), las confirmaciones de hotel, los "Reserva de viaje hacia…" de NUBA/Sabre, Boletos Nuba México (emisiones y reembolsos), Connect/Egresos (pagos) y Nuba Core (justificantes).

## 4. Consolidar por viaje
- Un proyecto por viaje: juntar los correos del mismo cliente o viaje y no repetir entradas.
- Nombre exacto: `DESTINOS_APELLIDO DEL CLIENTE_FECHAS`. Si falta un dato, usar un marcador visible como `[CLIENTE NO INDICADO]`, `[FECHA DE REGRESO NO INDICADA]` o `[FECHAS NO INDICADAS]`. Nunca inventar.
- Pendientes por viaje: vuelos, hoteles, traslados, actividades, pagos, documentos (pasaportes, visas, SSR DOCS) y seguimientos con el cliente o proveedor.
- Estado de cada pendiente:
  - **Completado** solo cuando llegó la confirmación (pago aplicado, reserva confirmada, boleto emitido).
  - **En curso** cuando ya se pidió y el proveedor está trabajando en ello.
  - **Pendiente** en cualquier otro caso.
- Fecha del pendiente: la fecha límite que dicen los correos. Si ya está vencido y no tiene fecha, usar hoy. Si no hay fecha posible, dejarlo vacío y decirlo en la nota.
- Marcar "Revisar ⚠️" y explicar en la nota cualquier ambigüedad: montos que no cuadran, dos números de confirmación, apellidos escritos distinto, vuelos que aparecen cancelados o correos cortados.
- Por defecto, dejar fuera los viajes que ya terminaron y no tienen nada abierto, y mencionarlos en el resumen.

## 5. Crear el calendario en Notion
- Página padre en borrador: "Calendario de Viajes NUBA – Maguis".
- Una sola base de datos, "Viajes y pendientes", con estas columnas:
  - `Nombre` (title)
  - `Tipo` (Viaje / Pendiente)
  - `Fecha` (date; para los viajes, el rango de salida y regreso)
  - `Estado` (Pendiente / En curso / Completado)
  - `Categoría` (Vuelos, Hotel, Traslado, Actividad, Pago, Documentos, Seguimiento)
  - `Nota` (texto)
  - `Revisar ⚠️` (casilla)
  - Auto-relación `Proyecto` ↔ `Pendientes`
- Fórmulas para ordenar por proximidad:
  - `Inicio del viaje`: `if(prop("Tipo") == "Viaje", dateStart(prop("Fecha")), dateStart(prop("Proyecto").map(current.prop("Fecha")).first()))`
  - `Orden`: `formatDate(prop("Inicio del viaje"), "YYYY-MM-DD") + " " + if(prop("Tipo") == "Viaje", prop("Nombre"), prop("Proyecto").map(current.prop("Nombre")).first()) + " " + if(prop("Tipo") == "Viaje", "0", "1")`
- El nombre de cada pendiente empieza con el viaje corto, por ejemplo "Marruecos · …" o "Maldivas · …".
- Vistas:
  1. **📋 Todo por proximidad** (tabla): `SORT BY "Orden" ASC, "Fecha" ASC`
  2. **📅 Calendario**: `CALENDAR BY "Fecha"`, mostrando Nombre, Proyecto, Estado y Tipo.
  3. **✅ Pendientes por viaje** (tabla): filtro Tipo = Pendiente, `SORT BY "Orden" ASC, "Fecha" ASC`. No agrupar por relación, porque Notion ordena los grupos alfabéticamente.
  4. **✈️ Viajes** (tabla): filtro Tipo = Viaje, `SORT BY "Orden" ASC`.
- Para varios criterios de orden, usar un solo `SORT BY` con comas. Varios `SORT BY` seguidos se sobrescriben.

## 6. Verificar y reportar
- Con una consulta SQL, contar los pendientes sin `Proyecto` (deben ser 0) y revisar con la vista de viajes que el orden sea correcto.
- En el resumen para Maguis: número de viajes y pendientes, lo más urgente de los próximos días, los viajes que se dejaron fuera y que la página está en borrador privado.
- Si Maguis cancela un viaje, mandar a la papelera el viaje y todos sus pendientes con `notion-delete-pages`, que se pueden recuperar.