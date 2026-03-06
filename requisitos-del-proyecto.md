

# Documento de Arquitectura del Sistema de Gestión de Órdenes y Entregas

## 1. Introducción  
En este documento se describe la arquitectura inicial de un sistema de una plataforma de eventos y boletería, incluyendo los requisitos funcionales, requisitos no funcionales en escenarios de calidad y restricciones que deben ser consideradas en el diseño del software.

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
| **US-01** | _Como **cliente** quiero registrarme para crear una cuenta y poder realizar compras._          | - El usuario debe registrarse con nombre, correo y contraseña.<br>- El sistema no permite correos duplicados.<br>- La contraseña de almacena usando hash seguro.<br>- El sistema confirma que el registro fue exitoso | **Sí cumple.**<br>**I:** Independiente de otras historias de usuario.<br>**N:** Es negociable (campos y filtros pueden ajustarse).<br>**V:** Aporta valor directo al cliente.<br>**E:** Es estimable.<br>**S:** puede completarse en 1 sprint.<br>**T:**. |
| **US-02** | _Como **cliente** quiero iniciar sesion en la plataforma para acceder a mi cuenta y realizar compras._                           | - El usuario debe ingresar con su correo y contraseña.<br>- El sistema valida las credenciales.<br>- Si las credenciales son correctas se genera un token de autenticacion.<br>- Si las crdenciales son incorrectas de muestra un mensaje de error. | **Sí cumple.**<br>**I:** Independiente de la historias de susuario US-01 .<br>**N:** El metodo de autenticacion puede ajustarse <br>**V:** Aporta valor directo al cliente.<br>**E:** Es estimable.<br>**S:** Puede completarse en 1 sprint.<br>**T:**                              |
| **US-03** | _Como **organizador** quiero crear un evento para ofrecerlo a los clientes en la plataforma._                                     | - El organizador debe poder ingresar nombre, descripcion, fecha y lugar.<br>- Se debe definir el aforo maximo del evento.<br>- El evento debe quedar registrado en el sistema.<br>- Se debe guardar quien creo el evento y cuando. | **Sí cumple.**<br>**I:** Independiente de otras historias de usuario.<br>**N:** Los campos del evento pueden ser modificados segun las necesidades del negocio.<br>**V:** Aporta valor directo al usuario y al negocio.<br>**E:** Es estimable.<br>**S:** Puede completarse en 1 sprint.<br>**T:**                        |
| **US-04** | _Como **organizador** quiero definir tipos de boletas, su precio y cantidad disponible para un envento, para que los clientes puedan comprarlas segun las opciones ofrecidas._ | - El organizador debe poder crear diferentes tipos de boletas (general, VIP, estudiante).<br>- Cada boleta debe tener un precio definido.<br>- El sistema debe validar que la cantidad total de boletas no supere el aforo del evento.| **Sí cumple.**<br>**I:** Independiente de la historia de usuario US-03.<br>**N:** Los tipos de boletas y precios puedes modificarse segun la necesidad del evento.<br>**V:** Aporta valor directo al usuario y al negocio.<br>**E:** Es estimable.<br>**S:** Puede completarse en 1 sprint.<br>**T:** . |
| **US-05** | _Como **cliente** quiero explorar el catalogo de eventos para encontrar eventos disponibles._ | - El sistema debe mostrar eventos activos.<br>- No deben aparecer eventos cancelados.<br>- Se debe mostrar nombre fecha y ciudad. | **Sí cumple.**<br>**I:** Independiente de otras historias de usuario.<br>**N:** La informacion a mostrar puede ajustarse segun necesidades.<br>**V:** Aporta valor directo al cliente y a la empresa.<br>**E:** Es estimable.<br>**S:** Puede completarse en 1 sprint.<br>**T:**  |
| **US-06** | _Como **cliente** quiero ver el detalle de un evento, precios y disponibilidad de boletas para decidir que boletas quiero comprar._ | - El sistema muestra descripcion completa del evento.<br>- El sistema muestra los tipos de boletas disponibles.<br>- El sistema muestra precios y cantidad disponible.<br>- El sistema muestra toda la informacion relacionada al evento. | **Sí cumple.**<br>**I:** Independiente de la historia de usuario US-05.<br>**N:** La informacion mostrada puede modificarse.<br>**V:** Permite al cliente tomar decision de compra aportanto valor directo al cliente y a la empresa.<br>**E:** Es estimable.<br>**S:** Puede completarse en 1 sprint.<br>**T:** . |
| **US-07** | _Como gerente quiero que el sistema genere una boleta digital con código QR único al aprobar el pago, para que el cliente pueda usarla como entrada al evento._ | - Se genera una boleta digital por cada unidad comprada al aprobarse el pago.<br>- Cada boleta tiene un código QR único e irrepetible.<br>- La boleta contiene: nombre del evento, fecha, tipo de boleta, nombre del cliente y QR.<br>- El cliente recibe un correo con su(s) boleta(s) adjunta(s) tras el pago aprobado.<br>- Si el envío del correo falla, la boleta queda pendiente de reenvío y se registra para reintento. | **Sí cumple.**<br>**I:** Puede desarrollarse de forma independiente dentro del proceso de generación de boletas, asumiendo como contexto que el pago ha sido aprobado.<br>**N:** El formato de la boleta y el canal de entrega son negociables.<br>**V:** Aporta valor al cliente al proporcionarle la boleta digital con código QR que confirma su compra y le permite acceder al evento.<br>**E:** Es estimable como generación de QR + envío de notificación.<br>**S:** Puede completarse en 1 Sprint.<br>**T:** Con una prueba de integración se puede validar que cada compra aprobada genera exactamente una boleta por unidad, que el QR es único, y que el correo queda encolado si el envío inicial falla. |
| **US-08** | _Como gerente quiero que el sistema genere una boleta digital con código QR único al aprobar el pago, para que el cliente pueda usarla como entrada al evento._ | - Se genera una boleta digital por cada unidad comprada al aprobarse el pago.<br>- Cada boleta tiene un código QR único e irrepetible.<br>- La boleta contiene: nombre del evento, fecha, tipo de boleta, nombre del cliente y QR.<br>- El cliente recibe un correo con su(s) boleta(s) adjunta(s) tras el pago aprobado.<br>- Si el envío del correo falla, la boleta queda pendiente de reenvío y se registra para reintento. | **Sí cumple.**<br>**I:** Puede desarrollarse de forma independiente dentro del proceso de generación de boletas, asumiendo como contexto que el pago ha sido aprobado.<br>**N:** El formato de la boleta y el canal de entrega son negociables.<br>**V:** Aporta valor al cliente al proporcionarle la boleta digital con código QR que confirma su compra y le permite acceder al evento.<br>**E:** Es estimable como generación de QR + envío de notificación.<br>**S:** Puede completarse en 1 Sprint.<br>**T:** Con una prueba de integración se puede validar que cada compra aprobada genera exactamente una boleta por unidad, que el QR es único, y que el correo queda encolado si el envío inicial falla. |

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
| **ID** | **Tipo de Restricción** | **Descripción** | **Evidencia (Cita del relato)** |
| ------ | ---------------------- | --------------- | ------------------------------- |
| **R-01** | Tiempo | *"Todo el desarrollo del proyecto debe realizar en un intervalo de 13 semanas. "* | *"Yo necesito que ustedes construyan el backend del MVP en 13 semanas, porque ya tengo conversaciones abiertas con organizadores de dosciudades y me están pidiendo una fecha para pruebas." * |
| **R-02** | "Herramientas" | "El sistema debe estar conectado a una pasarela de pago real y un sandbox que permita una visualizacion completa de los procesos" | *"Por eso yo quiero que el sistema esté conectado con una pasarela de pagos real, aunque sea en modo pruebas. No quiero simulaciones “de mentiras”; necesito por lo menos un sandbox real donde se vea el flujo, estados, callbacks, confirmaciones, etc."* |
| **R-03** | Tecnológica | "El" sistema no debe implementar recomendaciones con IA ni contar con App moviles nativas" | "No necesito motor de recomendaciones con IA, ni app móvil nativa, ni contabilidad completa. Necesito lo esencial, pero bien hecho, porque esto va a estar de cara a clientes reales." |
| **R-04** | Infraestructura | "El Backend del proyecto debe estar construido bajo la arquitectura de MicroServicios" | "En la empresa vamos a tener equipos distintos trabajando, así que necesito que esté separado en servicios con responsabilidades claras." |
| **R-05** | Seguridad | "Las bases de datos de los Servicios deben estar aisladas" | "No quieroque un servicio toque la base de datos de otro como atajo, porque después eso no lomantiene nadie." |
| **R-06** | Tecnológica |"" | "" |
| **R-07** | Infraestructura | El sistema debe desplegarse en un entorno orquestado con **Kubernetes**, permitiendo escalabilidad automática y alta disponibilidad. | *“Despliegue en Kubernetes”* |
| **R-08** | Herramientas | Se debe implementar un pipeline de **CI/CD con GitHub Actions**, para integración y despliegue automatizado. | *“CI/CD con GitHub Actions.”* |
| **R-09** | Herramientas | Las pruebas automatizadas deben realizarse con **JUnit, Mockito y Testcontainers**, asegurando cobertura de los módulos críticos. | *“Pruebas automatizadas con JUnit, Mockito y Testcontainers.”* |
| **R-10** | Herramientas | La observabilidad del sistema debe realizarse con **Prometheus y Grafana**, incluyendo logs centralizados y alertas automáticas ante errores o caídas de servicios. | *“Monitoreo con Prometheus y Grafana, más logs centralizados y alertas.”* |
| **R-11** | Tiempo | El proyecto debe ejecutarse en un período de **14 semanas**, correspondiente a la duración del semestre. | *“El semestre dura 14 semanas, con sprints de 2 semanas.”* |
| **R-12** | Proceso | La metodología de trabajo será **SCRUM**, con sprints de 2 semanas, incluyendo **daily stand-ups por Slack y revisión (review) al final de cada sprint**. | *“Daily stand-up en Slack y review cada sprint.”* |
| **R-13** | Proceso | Todo el trabajo debe registrarse y evidenciarse en un **tablero y repositorio** de manera actualizada y centralizada. | *“Todo debe quedar en tablero y repositorio con evidencia.”* |

>  **Tipos de restricciones:**  
> - **Tecnológicas:** Lenguajes, frameworks o herramientas que deben utilizarse.  
> - **De negocio:** Normativas o estándares de la empresa.  
> - **Regulatorias:** Cumplimiento de normativas legales o de seguridad.  
> - **De infraestructura:** Limitaciones en hardware, red o almacenamiento.  


