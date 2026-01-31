# Tecmax App – Estructura Lógica Base (Documento Maestro)

Este documento define la estructura lógica y la base lógica del sistema Tecmax App.
Es la referencia principal del proyecto y gobierna la coherencia entre visión,
procesos, datos, requerimientos y futuras implementaciones.

---

## 1. Principio estructural del sistema

Tecmax App se organiza bajo el siguiente eje central:

Empresa → Cliente → Equipo → Servicio Técnico → Insumos

El **contador** es un dato obligatorio y transversal a todo el sistema.
No existe servicio técnico ni instalación de insumos sin registro de contador.

---

## 2. Entidades raíz y relaciones

### Empresa
- Existe una sola empresa por instalación.
- Es la raíz del sistema.

Relación:
Empresa (1) → Clientes (N)

---

### Cliente
- Identificado de forma única por cédula o NIT.
- No puede existir sin una empresa.

Relación:
Cliente (1) → Equipos (N)

---

### Equipo
- Pertenece a un único cliente.
- Mantiene historial técnico independiente.
- Se identifica por número de serie dentro del cliente.

Relación:
Equipo (1) → Servicios Técnicos (N)

---

### Servicio Técnico
- Representa una intervención técnica sobre un equipo.
- El contador es obligatorio al inicio del servicio.

Relaciones:
Servicio Técnico (1) → Insumos (N)  
Servicio Técnico (1) → Documentos (N)

---

## 3. Insumos y cotizaciones

### Insumo
- Elemento utilizado, cambiado o evaluado durante un servicio técnico.
- Puede ser consumible o no consumible.

Estados:
- Cambiado
- No cambiado (pasa a cotización)

Relación:
Insumo (1) → Cotización (0..1)

---

### Cotización
- Solo existe si el insumo no fue cambiado.
- Permite seguimiento y recordatorios.
- Puede cerrarse cuando el insumo se cambia.

Relación:
Cotización (1) → Insumo (1)

---

## 4. Historial técnico

### Historial de Insumo
- Registra cambios reales de insumos.
- Incluye marca, fecha y contador del cambio.
- Permite calcular durabilidad por contador.

Relación:
Equipo (1) → Historial de Insumos (N)

Este historial es información interna del técnico y no se muestra al cliente.

---

## 5. Venta de Tóner

### Venta de Tóner
- Es un caso especial de servicio técnico.
- Puede generar historial si se instala.
- Puede quedar como backup si no se instala.

Relación:
Venta de Tóner (1) → Servicio Técnico (1)

---

## 6. Documentos

### Documento
- Generado a partir de un servicio técnico.
- Puede ser reporte técnico o factura.

Relación:
Servicio Técnico (1) → Documentos (N)

---

## 7. Reglas estructurales no negociables

- No existen servicios sin equipo.
- No existen equipos sin cliente.
- No existen clientes sin empresa.
- Todo servicio requiere contador obligatorio.
- El historial técnico no se elimina.
- Las cotizaciones no se pierden, se cierran o evolucionan.
- La información interna del técnico no siempre se refleja en documentos PDF.

---

## 8. Rol del documento

Este documento actúa como:
- Base lógica del sistema.
- Referencia estructural única del proyecto.
- Punto de alineación entre visión, procesos, datos y requerimientos.

No describe interfaz gráfica ni tecnología de implementación.
