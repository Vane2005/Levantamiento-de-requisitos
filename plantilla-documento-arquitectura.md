

# Documento de Arquitectura del Sistema de Gestión de Órdenes y Entregas

## 1. Introducción  
En este documento se describe la arquitectura inicial de un sistema de gestión de órdenes y entregas, incluyendo los requisitos funcionales, requisitos no funcionales en escenarios de calidad y restricciones que deben ser consideradas en el diseño del software.

**Integrantes:** _[Alejandra Osorio Giraldo - 2266128]_
                _[Santiago Useche Tascón - 2266200]_
                _[Alejandro Garzón - 2266088]_
                _[Vanessa Duran Mona - 2359394]_
                _[Miguel Angel Arboleda - 2160253]_
                _[Jose David Marmol - 2266370]_

---

## 2. Requisitos Funcionales  
Los requisitos funcionales se presentan en forma de **historias de usuario**, especificando los **criterios de aceptación**.

### **Historias de Usuario**

| **ID**    | **Historia de Usuario**                                                                                                                                      | **Criterios de Aceptación**                                                                                                                                                                                                                       | **INVEST**                                                                                                                                                                                                         |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **US-01** | _Como **cliente** quiero consultar el catálogo de productos con precio y disponibilidad, para saber qué puedo comprar antes de realizar un pedido._          | - El sistema muestra nombre, categoría, precio y stock disponible por producto.<br>- No se muestran productos con stock cero como disponibles.<br>- La información se actualiza cuando cambia el inventario.<br>- Los productos pueden filtrarse por categoría. | **Sí cumple.**<br>**I:** Independiente de otras funcionalidades.<br>**N:** Es negociable (campos y filtros pueden ajustarse).<br>**V:** Aporta valor directo al cliente.<br>**E:** Es estimable.<br>**S:** Tarea pequeña y acotada.<br>**T:** Se puede testear validando que datos mostrados coincidan con inventario real. |
| **US-02** | _Como **cliente** quiero seleccionar productos y agregarlos al carrito de compra, para reservar los productos que deseo adquirir._                           | - Solo se pueden agregar productos con stock disponible.<br>- El sistema muestra el resumen del carrito con cantidad, precio unitario y total.<br>- El cliente puede modificar cantidades o eliminar productos del carrito.<br>- El pedido queda en estado **"Pendiente de pago"** al confirmarse. | **Sí cumple.**<br>**I:** Independiente del módulo de pagos.<br>**N:** Los detalles del carrito son negociables.<br>**V:** Aporta valor directo al cliente.<br>**E:** Es estimable.<br>**S:** Funcionalidad acotada.<br>**T:** Se puede probar agregando, modificando y eliminando ítems.                             |
| **US-03** | _Como **cliente** quiero realizar el pago de mi pedido, para confirmar la compra y continuar con el proceso de entrega._                                     | - Solo pedidos en estado **"Pendiente de pago"** pueden procesarse.<br>- Si el pago es exitoso, el estado cambia a **"Pago aprobado"**.<br>- Si el pago falla, el pedido permanece en **"Pendiente de pago"** y se notifica al cliente.<br>- Se registra fecha, hora y resultado del pago.<br>- El pago puede ser simulado para efectos del proyecto. | **Sí cumple.**<br>**I:** Independiente del módulo de catálogo.<br>**N:** El método de pago puede cambiar o simularse.<br>**V:** Permite confirmar la compra.<br>**E:** Es estimable.<br>**S:** Funcionalidad específica y acotada.<br>**T:** Se puede probar con escenarios de éxito y fallo.                         |
| **US-04** | _Como **cliente** quiero consultar el estado actual de mi pedido y recibir notificaciones ante cambios, para estar informado sin necesidad de contactar soporte._ | - El cliente puede ver el estado actual de su pedido: **Pendiente de pago / Pago aprobado / Empacado / En ruta / Entregado / Cancelado**.<br>- El sistema emite una notificación cuando el estado cambia.<br>- El historial de cambios de estado queda visible para el cliente.<br>- La información se actualiza con una frecuencia máxima de 30 segundos cuando el pedido está **"En ruta"**. | **Sí cumple.**<br>**I:** Independiente del módulo de pagos y catálogo.<br>**N:** El canal de notificación es negociable.<br>**V:** Reduce llamadas a soporte y mejora la experiencia.<br>**E:** Es estimable.<br>**S:** Funcionalidad concreta de consulta y notificación.<br>**T:** Se puede validar verificando que los estados y notificaciones correspondan a los cambios registrados. |
| **US-05** | _Como **administrador de inventario** quiero actualizar el stock de productos cuando ingrese mercancía o haya ajustes, para evitar inconsistencias entre el sistema y la bodega._ | - Permite aumentar o disminuir el stock de un producto.<br>- No se permiten valores de stock negativos.<br>- El cambio se refleja de forma inmediata en el catálogo visible al cliente.<br>- Se registra un log del ajuste con usuario, fecha, hora y motivo del cambio. | **Sí cumple.**<br>**I:** Independiente del módulo de pagos y entregas.<br>**N:** Los campos del ajuste son negociables.<br>**V:** Evita sobreventas e inconsistencias.<br>**E:** Es estimable.<br>**S:** Tarea puntual y acotada.<br>**T:** Se puede validar verificando que el catálogo refleje el nuevo stock inmediatamente. |
| **US-06** | _Como **administrador de inventario** quiero gestionar el catálogo de productos (crear, editar, desactivar), para mantener actualizada la oferta disponible para los clientes._ | - Se pueden crear productos con nombre, categoría, precio y stock inicial.<br>- Se pueden editar los datos de un producto existente.<br>- Se puede desactivar un producto para que no sea visible en el catálogo sin eliminarlo.<br>- No se puede desactivar un producto si tiene pedidos activos asociados. | **Sí cumple.**<br>**I:** Independiente del módulo de entregas.<br>**N:** Los campos del producto son negociables.<br>**V:** Permite mantener el catálogo consistente y actualizado.<br>**E:** Es estimable.<br>**S:** CRUD acotado sobre el catálogo.<br>**T:** Se puede validar creando, editando y desactivando productos y verificando el resultado en el catálogo. |
| **US-07** | _Como **operador de entregas** quiero asignar pedidos aprobados a repartidores, para organizar la distribución de manera ordenada._                          | - Solo pedidos en estado **"Empacado"** pueden asignarse a un repartidor.<br>- Cada pedido tiene asignado un único repartidor a la vez.<br>- Se registra fecha y hora de la asignación.<br>- Es posible reasignar el pedido a otro repartidor si es necesario, quedando registro del cambio. | **Sí cumple.**<br>**I:** No depende directamente del módulo de pagos.<br>**N:** El criterio de asignación puede cambiar.<br>**V:** Mejora la organización del reparto.<br>**E:** Es estimable.<br>**S:** Funcionalidad específica de asignación.<br>**T:** Se puede probar asignando y reasignando pedidos.             |
| **US-08** | _Como **repartidor** quiero actualizar el estado del pedido y registrar novedades durante la entrega, para informar el progreso y dejar trazabilidad de incidentes._ | - El repartidor puede cambiar el estado a **"En ruta"** y **"Entregado"**.<br>- Puede registrar novedades (dirección errónea, cliente ausente, etc.) con descripción, fecha y hora.<br>- Las novedades quedan registradas en el historial del pedido.<br>- Al marcar un pedido como **"Entregado"**, el sistema descarta novedades pendientes no resueltas o las escala al operador. | **Sí cumple.**<br>**I:** Independiente del catálogo y del módulo de pagos.<br>**N:** Los tipos de novedad pueden ampliarse.<br>**V:** Aporta trazabilidad y visibilidad a la operación.<br>**E:** Es estimable.<br>**S:** Tarea concreta sobre estados del pedido.<br>**T:** Se puede validar cambiando estados y registrando novedades.                                          |

>  **Instrucciones:**  
> - Completar al menos **6 historias de usuario**.  
> - Asegurar que cada historia tenga criterios de aceptación claros y verificables.  

---

## 3. Requisitos no funcionales como escenarios de Calidad  
Los requisitos de calidad se presentan en forma de **historias de calidad**, siguiendo la estructura de Len Bass.

### **Historias de Calidad**
| **ID**    | **Fuente**         | **Estímulo**         | **Artefactos**         | **Entorno**         | **Respuesta**         | **Medida de Respuesta**         |
| --------- | ------------------ | -------------------- | ---------------------- | ------------------- | --------------------- | ------------------------------- |
| **RQ-01** | Usuario registrado | Solicita agregar un producto a su carrito | Backend y bases de datos | Operacion normal |El sistema procesa la solicitud y agrega el producto al modulo de carrito | No debe de superar los 5 segundos en tiempo de respuesta |
| **RQ-02** | Mayor cantidad de usuarios concurrentes | Aumento significativo en la carga de usuarios | Infraestructura en nube | Uso excesivo | El sistema debe escalar adecuadamente añadiendo instancias | El tiempo de respuesta no debe de verse afectado de forma tan significativa por un tiempo prolongado |
| **RQ-03** | Usuario desconocido | Intenta acceder a informacion personal de otros usuarios | Bases de datos y modulo de autenticacion | Operacion normal | El sistema bloquea el acceso | Se deben de rechazar el 100% de los accesos no autorizados y registrarlos en los log|
| **RQ-04** | Caídas en servicios críticos | Una instancia de un microservicio se cae | Microservicios desplegados en Kubernetes | Operación normal o pico | Redirigiendo a instancias sanas y generando alertas | Disponibilidad mínima del 99% mensual. Redirección en menos de 5 segundos. Alerta generada en menos de 1 minuto |
| **RQ-05** | Falta de monitoreo | Errores repetitivos, tiempos altos, caída de un servicio, cola creciendo sin control | Microservicios y API Gateway | Entorno de uso normal o pico | El sistema debe registrar logs, exponer métricas y generar alertas automáticas | Métricas expuestas cada 15 segundos. Alertas generadas si el tiempo promedio supera 2 minutos continuos |
| **RQ-06** | Uso de mensajería/eventos | Falla temporal en el procesamiento de un mensaje | Microservicios | Entorno de uso normal | El mensaje debe reintentarse automáticamente o enviarse a una cola de pendientes sin perderse ni duplicarse | 0% pérdida de mensajes. 0% duplicación que afecte el negocio. Reintento automático en máximo 3 intentos antes de enviarse a cola de pendientes |

>  **Instrucciones:**  
> - Completar al menos **6 historias de calidad**, alineadas con atributos clave como **rendimiento, escalabilidad y seguridad**.  
> - Definir cómo se medirá la respuesta esperada ante la situación planteada.  

---

## 4. Restricciones del Sistema  
Las restricciones establecen **limitaciones** en la arquitectura del sistema, ya sean tecnológicas, de negocio, regulatorias o de infraestructura.

### **Lista de Restricciones**
| **Tipo de Restricción** | **Descripción**                                                                                                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _[Agregar otro tipo]_   | _[Describir la restricción]_                                                                                                                                             |

>  **Tipos de restricciones:**  
> - **Tecnológicas:** Lenguajes, frameworks o herramientas que deben utilizarse.  
> - **De negocio:** Normativas o estándares de la empresa.  
> - **Regulatorias:** Cumplimiento de normativas legales o de seguridad.  
> - **De infraestructura:** Limitaciones en hardware, red o almacenamiento.  



