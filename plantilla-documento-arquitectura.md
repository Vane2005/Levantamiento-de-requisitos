

# Documento de Arquitectura del Sistema de Gestión de Órdenes y Entregas

## 1. Introducción  
Este documento describe la arquitectura inicial del sistema de gestión de órdenes y entregas, incluyendo requisitos funcionales, requisitos de calidad y restricciones que deben ser consideradas en el diseño del software.

**Equipo:** _[Nombre del equipo]_  
**Integrantes:** _[Alejandra Osorio Giraldo - 2266128]_  
**Fecha:** _[DD/MM/AAAA]_  

---

## 2. Requisitos Funcionales  
Los requisitos funcionales se presentan en forma de **historias de usuario**, especificando los **criterios de aceptación**.

### **Historias de Usuario**
| **ID**    | **Historia de Usuario**               | **Criterios de Aceptación**              | INVEST                                      |
| --------- | -------------------------------       | ---------------------------------------- | ------------------------------------------- |
| **US-01** | _[Como cliente quiero consultar       |_[El sistema muestra nombre, categoria    |  _[Si cumple.                               |
|           |    el catalogo de productos con       | precio y stock disponible, no se muestra | I: independiente a otras funcionalidades    |
|           |    costo y disponibilidad, para       | productos con stock cero como disponible | N: Es negociable                            |
|           |    saber que puedo comprar antes      | la informacion se actualiza cuando       | V: Aporta valor directo al cliente          |
|           |    de realizar un pedido]_            | cambia el inventario.]_                  | E: Es estimable                             |
|           |                                       |                                          | S: Es una tarea pequeña                     |
|           |                                       |                                          | T: Se puede testear validando datos]_       |
----------------------------------------------------------------------------------------------------------------------------------------
| **US-02** | _[Como cliente quiero                 | -[Solo se incluyen productos con stock   | _[Si cumple.                                |
|           |    seleccionar productos y agregarlos | disponible, el pedido queda en estado    | I: independiente a otras funcionalidades    |
|           |    al carrito de compra para resevar  | "pendiente de pago", el sistema calcula  | N: Es negociable                            |
|           |    los productos que quiero]_         | el valor del pedido.]_                   | V: Aporta valor directo al cliente          |
|           |                                       |                                          | E: Es estimable                             |
|           |                                       |                                          | S: Es una tarea pequeña                     |
|           |                                       |                                          | T: Se pueden realizar pruebas]_             |
----------------------------------------------------------------------------------------------------------------------------------------------
| **US-03** | _[Como cliente quiero realizar el     | -[Solo pedidos en estado "Pendiente de   | _[Si cumple.                                |
|           |    pago de mi pedido para             | pago" pueden pagarse.                    | I: Es una tarea independiente               |
|           |    confirmar la compra y continuar    | Si el pago es exitoso el estado cambia   | N: Puede cambiar el metodo de pago          |
|           |    con el proceso de entrega]_        | a "Pago aprobado".                       | V: Permite confirmar la compra              |
|           |                                       | Si el pago falla permanece en            | E: Es estimable                             |
|           |                                       | "Pendiente de pago".                     | S: Es una funcionalidad especifica          |
|           |                                       | Se registra fecha y resultado del pago.]_| T: Se puede probar con exito y fallo]_      |
----------------------------------------------------------------------------------------------------------------------------------------------
| **US-04** | _[Como administrador de inventario    | -[Permite aumentar o disminuir stock.    | _[Si cumple.                                |
|           |    quiero actualizar el stock cuando  | No permite valores negativos.            | I: Independiente del modulo de pagos        |
|           |    ingrese nueva mercancia o haya     |  El cambio se refleja inmediatamente     | N: Es negociable                            |
|           |    ajustes para evitar inconsistencias]_ |en el catalogo.]_                      | V: Evita sobreventas                        |
|           |                                       |                                          | S: Es una tarea puntual                     |
|           |                                       |                                          | T: Se puede validar con pruebas]_           |
----------------------------------------------------------------------------------------------------------------------------------------------
| **US-05** | _[Como operador de entregas quiero    | -[Solo pedidos en estado "Empacado"      | _[Si cumple.                                |
|           |    asignar pedidos aprobados a        | pueden asignarse.                        | I: No depende directamente del pago         |
|           |    repartidores para organizar la     | Cada pedido tiene un solo repartidor.    | N: El criterio puede cambiar                |
|           |    distribucion de manera ordenada]_  | Se registra fecha y hora de asignacion.  | V: Mejora la organizacion                   |
|           |                                       | Permite reasignacion si es necesario.]_  | E: Es estimable                             |
|           |                                       |                                          | S: Es funcionalidad especifica              |
|           |                                       |                                          | T: Se puede probar asignando]_              |
----------------------------------------------------------------------------------------------------------------------------------------------
| **US-06** | _[Como repartidor quiero actualizar   | -[Puede cambiar el estado a "En ruta" y  | _[Si cumple.                                |
|           |    el estado del pedido y registrar   | "Entregado".                             | I: Independiente del catalogo               |
|           |    novedades para informar el         | Puede registrar novedades.               | N: Puede ampliarse                          |
|           |    progreso de la entrega]_           | Se registra fecha y hora del cambio.     | V: Aporta trazabilidad                      |
|           |                                       | Las novedades quedan en el historial.]_  | E: Es estimable                             |
|           |                                       |                                          | S: Es una tarea concreta                    |
|           |                                       |                                          | T: Se puede validar cambiando estados]_     |



>  **Instrucciones:**  
> - Completar al menos **6 historias de usuario**.  
> - Asegurar que cada historia tenga criterios de aceptación claros y verificables.  

---

## 3. Requisitos de Calidad  
Los requisitos de calidad se presentan en forma de **historias de calidad**, siguiendo la estructura de Len Bass.

### **Historias de Calidad**
| **ID**    | **Fuente**         | **Estímulo**         | **Artefactos**         | **Entorno**         | **Respuesta**         | **Medida de Respuesta**         |
| --------- | ------------------ | -------------------- | ---------------------- | ------------------- | --------------------- | ------------------------------- |
| **RQ-01** | _[Agregar fuente]_ | _[Agregar estímulo]_ | _[Agregar artefactos]_ | _[Agregar entorno]_ | _[Agregar respuesta]_ | _[Agregar medida de respuesta]_ |
| **RQ-02** | _[Agregar fuente]_ | _[Agregar estímulo]_ | _[Agregar artefactos]_ | _[Agregar entorno]_ | _[Agregar respuesta]_ | _[Agregar medida de respuesta]_ |
| **RQ-03** | _[Agregar fuente]_ | _[Agregar estímulo]_ | _[Agregar artefactos]_ | _[Agregar entorno]_ | _[Agregar respuesta]_ | _[Agregar medida de respuesta]_ |

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



