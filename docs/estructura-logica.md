# Tecmax App – Estructura Lógica Base (Documento Maestro)

Este documento define la estructura lógica y la base conceptual del sistema Tecmax App.
Es la referencia principal del proyecto y gobierna la coherencia entre visión,
procesos, datos, requerimientos y la implementación futura.

---

## 1. Principio estructural del sistema

Tecmax App se organiza bajo el siguiente eje central:

Empresa → Cliente → Equipo → Servicio Técnico → Insumos

El **contador** es un dato obligatorio y transversal.
No existe servicio técnico ni instalación de insumos sin registro de contador.

El sistema está diseñado para operar en modo **offline-first**,
con sincronización periódica a la nube como respaldo y consolidación.

---

## 2. Entidades raíz y relaciones

### Empresa
- Existe una sola empresa por instalación.
- Es la raíz del sistema y provee contexto global.

La empresa gestiona:
- identidad (nombre, logo, NIT)
- técnicos
- proveedores
- preferencias técnicas
- carpeta raíz de almacenamiento
- respaldo en la nube

Relación:
Empresa (1) → Clientes (N)  
Empresa (1) → Técnicos (N)  
Empresa (1) → Proveedores (N)

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

## 3. Servicios técnicos

### Servicio Técnico
- Representa una intervención técnica sobre un equipo.
- Siempre está asociado a un técnico.
- El contador es obligatorio.
- El servicio es la **unidad real de trabajo y facturación**.

Relaciones:
Servicio Técnico (1) → Insumos (N)  
Servicio Técnico (1) → Documentos (N)

El **precio final** que aparece en la factura se define y se congela
únicamente en el servicio donde el insumo se instala.

---

## 4. Técnicos y roles

### Técnico
- Puede existir más de un técnico por empresa.
- Cada técnico trabaja de forma independiente y offline.
- El técnico es un **dato del servicio**, no una estructura de carpetas.

Roles:
- Técnico de campo: registra servicios e insumos.
- Técnico central / jefe: cotiza, habla con proveedores y controla numeración.

Relación:
Técnico (1) → Servicios Técnicos (N)

---

## 5. Insumos y cotizaciones

### Insumo
- Elemento utilizado o evaluado durante un servicio técnico.
- Puede ser consumible o no consumible.

Estados:
- Cambiado
- No cambiado (genera cotización)

Relación:
Insumo (1) → Cotización (0..1)

---

### Cotización
- Solo existe para insumos no cambiados.
- Registra precios del proveedor y acuerdos preliminares con el cliente.
- No genera facturación directa.

Reglas:
- La cotización **no define el precio final de la factura**.
- El precio final se guarda solo en el servicio donde se instala el insumo.

Relación:
Cotización (1) → Insumo (1)

---

## 6. Historial técnico

### Historial de Insumo
- Registra cambios reales de insumos.
- Incluye marca, fecha y contador del cambio.
- Permite análisis de durabilidad y calidad.

Relación:
Equipo (1) → Historial de Insumos (N)

Este historial es información interna del técnico y no se muestra al cliente.

---

## 7. Venta de Tóner

### Venta de Tóner
- Es un caso especial de servicio técnico.
- Puede generar historial si se instala.
- Puede quedar como backup si no se instala.

Relación:
Venta de Tóner (1) → Servicio Técnico (1)

---

## 8. Documentos

### Documento
- Generado a partir de un servicio técnico.
- Puede ser reporte técnico o factura.

Relación:
Servicio Técnico (1) → Documentos (N)

Los documentos utilizan información de la empresa (nombre y logo),
pero no exponen toda la información técnica interna.

---

## 9. Sincronización y numeración

- Cada técnico trabaja offline en su dispositivo.
- Los servicios se crean sin numeración global.
- Cada 12 horas (o manualmente) se sincroniza con la nube.
- En la sincronización:
  - se comparan clientes, equipos y servicios
  - se asigna un **ID global único** al servicio
  - se consolida el historial central

La carpeta central de la empresa es la **única fuente de verdad**.

---

## 10. Reglas estructurales no negociables

- No existen servicios sin equipo.
- No existen equipos sin cliente.
- No existen clientes sin empresa.
- Todo servicio requiere contador obligatorio.
- La numeración de servicios es global y centralizada.
- El técnico no define estructura de carpetas.
- Las cotizaciones no generan facturas automáticamente.
- El historial técnico nunca se elimina.

---

## 11. Rol del documento

Este documento actúa como:
- base lógica del sistema
- referencia estructural única
- punto de alineación entre visión, procesos, datos y código

No describe interfaz gráfica ni tecnología específica.
