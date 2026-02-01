# Tecmax App – Estructura Lógica Base (Documento Maestro)

Este documento define la estructura lógica y los principios fundamentales
del sistema Tecmax App.  
Es el documento rector del proyecto y sirve como referencia para alinear
visión, procesos, modelo de datos, flujos y desarrollo de software.

---

## 1. Principio estructural del sistema

Tecmax App se organiza bajo el siguiente eje principal:

Empresa → Cliente → Equipo → Servicio Técnico → Insumos

El **servicio técnico** es la unidad base del sistema.
El **contador** es un dato obligatorio y transversal.

El sistema está diseñado bajo un enfoque **offline-first**, con
sincronización periódica a la nube para consolidación y respaldo.

---

## 2. Empresa (raíz del sistema)

Existe **una sola empresa por instalación**.
La empresa es la raíz lógica y organizativa del sistema.

La empresa gestiona:

- Identidad:
  - nombre
  - logo
  - NIT
  - teléfono
  - correo electrónico
- Técnicos
- Proveedores
- Preferencias técnicas (ej. contador obligatorio)
- Tipos de equipos que atiende
- Carpeta raíz de almacenamiento
- Respaldo en la nube (Drive)

Relaciones:
- Empresa (1) → Clientes (N)
- Empresa (1) → Técnicos (N)
- Empresa (1) → Proveedores (N)

---

## 3. Técnicos y roles

Una empresa puede tener **varios técnicos**.

Cada técnico:
- se autentica con clave
- trabaja offline en su dispositivo
- queda registrado en cada servicio que realiza

Roles:
- Técnico de campo: registra servicios e insumos.
- Técnico central / jefe: cotiza, gestiona proveedores y controla numeración.

El técnico es **un dato del servicio**, no una estructura de carpetas.

---

## 4. Tipos de equipos atendidos

La empresa define una **lista configurable de tipos de equipos** que atiende
(ej. fotocopiadora, impresora, computador, televisor, etc.).

Estos tipos:
- se configuran al crear la empresa
- se pueden editar posteriormente
- se utilizan como lista controlada al crear equipos

Esto garantiza consistencia y evita errores de clasificación.

---

## 5. Cliente

El cliente representa a la persona o empresa que recibe el servicio técnico.

Reglas:
- Identificado de forma única por cédula o NIT.
- No puede existir sin una empresa.
- No se permiten clientes duplicados.

Relación:
- Cliente (1) → Equipos (N)

---

## 6. Equipo

El equipo representa un dispositivo atendido por la empresa.

Características:
- Pertenece a un único cliente.
- Se identifica por número de serie (único por cliente).
- Mantiene historial técnico independiente.

Relación:
- Equipo (1) → Servicios Técnicos (N)

---

## 7. Servicio Técnico

El servicio técnico representa una intervención real sobre un equipo.

Reglas:
- Siempre asociado a un técnico.
- El contador es obligatorio.
- El servicio es la **unidad real de trabajo, historial y facturación**.

Relaciones:
- Servicio Técnico (1) → Insumos (N)
- Servicio Técnico (1) → Documentos (N)

El **precio final** que aparece en la factura se define y se congela
únicamente en el servicio donde el insumo se instala.

---

## 8. Insumos y cotizaciones

### Insumo
Elemento utilizado o evaluado durante un servicio técnico.

Estados posibles:
- Cambiado
- No cambiado (genera cotización)

---

### Cotización

La cotización existe solo para insumos **no cambiados**.

Reglas:
- Solo el técnico central puede cotizar.
- Registra precios del proveedor y acuerdos preliminares.
- **No genera facturación directa**.
- El precio de la cotización no es el precio final de factura.

La cotización queda pendiente hasta que el insumo se instale
en un servicio posterior.

---

## 9. Sincronización y numeración

El sistema funciona de forma distribuida:

- Cada técnico trabaja offline.
- Los servicios se crean sin numeración global.
- Cada 12 horas (o manualmente) se sincroniza con la nube.

Durante la sincronización:
- Se comparan clientes, equipos y servicios.
- Se asigna un **ID global único** a los servicios nuevos.
- Se consolida el historial central.

La carpeta central de la empresa es la **única fuente de verdad**.

---

## 10. Informes, arqueo y análisis mensual

Tecmax App incluye un sistema de informes internos orientados al control técnico,
operativo y financiero del negocio.  
Estos informes no crean información nueva, sino que analizan datos existentes.

### Principio fundamental
- El servicio técnico es la unidad base.
- El historial de insumos es la memoria.
- Los informes son lecturas y agrupaciones.

---

### Informes mensuales internos

El sistema permite generar, por mes:

- Informe de insumos cambiados (general)
- Informe de insumos no cambiados / cotizados
- Informe de ventas de tóner
- Informe de servicios realizados
- Informe de facturación total
- Informe por técnico (opcional)

Estos informes se generan bajo demanda y no afectan la operación diaria.

---

### Arqueo técnico y financiero

El arqueo mensual permite:
- validar lo facturado vs lo ejecutado
- analizar frecuencia de fallas
- identificar insumos más utilizados
- preparar compras por volumen

---

### Vistas por cliente y por equipo

El sistema permite visualizar:
- insumos cambiados por cliente
- insumos cambiados por equipo
- historial completo por máquina

Estas vistas son consultas lógicas basadas en servicios e historial,
no estructuras de carpetas independientes.

---

### Reglas importantes

- No se crean carpetas duplicadas de insumos.
- No existen carpetas separadas de repuestos cambiados o no cambiados.
- Todo insumo vive dentro de un servicio y tiene un estado.
- Los informes no modifican datos originales.

---

## 11. Reglas estructurales no negociables

- No existen servicios sin equipo.
- No existen equipos sin cliente.
- No existen clientes sin empresa.
- Todo servicio requiere contador obligatorio.
- La numeración de servicios es global y centralizada.
- El técnico no define estructura de carpetas.
- Las cotizaciones no generan facturas automáticamente.
- El historial técnico nunca se elimina.

---

## 12. Rol del documento

Este documento es la referencia lógica principal del sistema.
Cualquier cambio estructural debe reflejarse aquí antes de
implementarse en código.
