# Portal Corporativo WordPress en AWS - Comercial Nova 🚀

## 👥 Integrantes y Roles
* **Carlos Santiago Bustamante Carpio** - Consultor de Infraestructura Cloud, Seguridad y Arquitecto de Redes

---

## 📋 1. Problema Planteado y Alcance
La empresa en crecimiento Comercial Nova requiere la implementación de su nuevo portal web corporativo y de contenidos utilizando WordPress. Los requerimientos del negocio exigen una solución integral en la nube que garantice los siguientes pilares fundamentales:
* **Alta Disponibilidad:** Resiliencia ante fallos de infraestructura distribuyendo la carga en múltiples zonas de disponibilidad.
* **Seguridad:** Aislamiento estricto de la base de datos en una red privada y control de accesos perimetrales por IP.
* **Scalabilidad:** Capacidad de absorber picos de demanda mediante políticas automáticas basadas en el consumo de CPU.
* **Control de Costos:** Una arquitectura eficiente y optimizada adaptada a los límites presupuestarios de AWS Academy.

El alcance del proyecto abarca el diseño, despliegue, aprovisionamiento automatizado, monitoreo operacional y análisis financiero de la infraestructura cloud.

---

## 🗺️ 2. Arquitectura Propuesta (Resumen del Diseño)
Nuestra arquitectura se basa en el patrón clásico de **Tres Capas (3-Tier Architecture)** altamente disponible, distribuida de forma redundante en dos Zonas de Disponibilidad (`us-east-1a` y `us-east-1b`) dentro de la región de AWS:

* **Capa de Redes y Perímetro (Público):** Aloja un **Application Load Balancer (ALB)** conectado a Internet Gateway para recibir el tráfico de los usuarios y distribuirlo uniformemente.
* **Capa de Aplicación / Cómputo (Privado):** Dos instancias **EC2** corriendo el stack web Apache + PHP que procesan WordPress. Al estar en subredes privadas, están protegidas del exterior y usan un **NAT Gateway** únicamente para la descarga segura de paquetes.
* **Capa de Datos (Privado):** Una instancia de base de datos relacional administrada con **Amazon RDS MySQL**, aislada en la red privada y accesible exclusivamente por los servidores web.

*(El diagrama lógico detallado se encuentra guardado en la ruta: `/arquitectura/diagrama.png`)*

---

## 🛠3. Servicios Cloud Utilizados y por qué

1. **Amazon VPC (Virtual Private Cloud):** Permite segmentar la red virtualmente para crear subredes públicas y privadas, aislando los recursos críticos del tráfico malicioso de internet.
2. **Amazon EC2 (Elastic Compute Cloud):** Proporciona la capacidad de cómputo elástica con instancias `t3.micro`/`t2.micro` que ejecutan el servidor web bajo el modelo de la capa gratuita.
3. **Application Load Balancer (ALB):** Distribuye de manera inteligente las solicitudes web HTTP de los clientes hacia las instancias saludables mediante comprobaciones de estado periódicas (*Health Checks*).
4. **Auto Scaling Group (ASG):** Garantiza la escalabilidad y el *autohealing* (recuperación automática) de la capa web ante aumentos imprevistos de la demanda mediante políticas simples de CPU.
5. **Amazon RDS MySQL:** Proporciona un motor de base de datos relacional robusto y completamente administrado, eliminando la carga operativa mediante parches y respaldos automáticos en caliente.
6. **Amazon S3 (Simple Storage Service):** Actúa como almacenamiento persistente y seguro con políticas no públicas y versionado activo para guardar copias de seguridad y medios del portal.
7. **AWS IAM (Identity and Access Management):** Implementa el principio de mínimo privilegio asignando roles de recursos seguros (`LabInstanceProfile`) a las instancias EC2 en lugar de usar claves de acceso estáticas.
8. **Amazon CloudWatch:** Proporciona observabilidad centralizada mediante la creación de un Dashboard métrico y alertas automáticas ante anomalías del sistema.

---

## 🚀 4. Pasos para Desplegar o Reproducir la Solución

1. **Despliegue de Red:** Crear 1 VPC propia con el bloque CIDR `10.0.0.0/16`. Configurar 2 subredes públicas y 2 privadas distribuidas equitativamente en 2 Zonas de Disponibilidad junto con un Internet Gateway y un NAT Gateway.
2. **Configuración de Seguridad:** 
   * Crear el grupo `Web-SG` abriendo los puertos HTTP (80) y HTTPS (443) al público, restringiendo SSH (22) a tu IP o Session Manager.
   * Crear el grupo `DB-SG` abriendo el puerto MySQL (3306) con acceso exclusivo para los recursos que tengan asignado el grupo `Web-SG`.
3. **Aprovisionamiento de la Base de Datos:** Crear un Subnet Group privado en RDS y lanzar la instancia MySQL configurando el nombre inicial de la base de datos como `wordpressdb` y habilitando respaldos automáticos.
4. **Lanzamiento de Servidores Web con Automatización:** Desplegar las instancias EC2 en las subredes privadas. En la sección de *Detalles Avanzados*, asignar el perfil IAM `LabInstanceProfile` y pegar el script de *User Data* para la instalación automatizada del stack LAMP junto con el archivo de Health Check.
5. **Enlace del Balanceador y Auto Scaling:** Crear un Target Group apuntando al archivo estático `/healthcheck.html` en el puerto 80, asociarlo al ALB público y configurar las políticas de escalamiento del Auto Scaling Group.
6. **Instalación Final:** Acceder al DNS Name provisto por el ALB, configurar el idioma de WordPress y enlazar el Endpoint de RDS con las credenciales maestras de la base de datos.

---

## 🔗 5. URL de Acceso y Evidencias de Funcionamiento
* **URL Pública del Portal Corporativo (ALB DNS):** `http://wordpress-alb-937337961.us-east-1.elb.amazonaws.com`

### Resumen de Evidencias Técnicas de AWS:
Todas nuestras capturas de pantalla reales del laboratorio académico se encuentran debidamente organizadas dentro de la ruta `/aws/evidencias/capturas_servicios/` e incluyen de manera obligatoria el nombre del estudiante y la hora del sistema:
* **Amazon VPC:** Captura de la VPC segmentada con las tablas de rutas y subredes funcionales.
* **Amazon EC2 & ALB:** Evidencia de los servidores web en estado *Running* y el Target Group reportando un estado enteramente **Healthy**.
* **Amazon RDS:** Captura de la instancia de base de datos disponible en red privada y su ventana de backup documentada.
* **Amazon S3:** Evidencia de la creación del bucket protegido con versión habilitada y la carga/recuperación del archivo de prueba.
* **WordPress en Ejecución:** Pantallas de la instalación completada con éxito, acceso al panel de administración y la primera entrada corporativa publicada en base de datos.
* **Amazon CloudWatch:** Evidencia del Dashboard de métricas operacionales combinado y la alarma de CPU configurada al 80%.

---

## ⚠️ 6. Limitaciones Conocidas y Mejoras Futuras (AWS Academy)
* **Limitación de Cuotas de VCPU:** El entorno cerrado de AWS Academy limita el número máximo de instancias simultáneas permitidas en el laboratorio. Debido a esto, el Auto Scaling Group se configuró de forma conservadora con un límite máximo de 4 instancias para evitar sobrecostos y denegaciones de la API de AWS.
* **Restricción de Permisos IAM:** El rol de laboratorio preconfigurado (`LabRole`) posee permisos restringidos que impiden la creación de tópicos de notificación SNS personalizados. Como mejora futura en un entorno empresarial real, las alarmas de CloudWatch se acoplarían directamente con alertas automáticas a canales de comunicación como Slack o correos automatizados mediante Amazon SNS.
* **Uso de un Solo NAT Gateway:** Por motivos estrictos de conservación de presupuesto dentro del laboratorio, se optó por desplegar un único NAT Gateway en lugar de colocar uno por zona de disponibilidad. Para un entorno real de Comercial Nova, se desplegaría redundancia completa de NAT Gateways para eliminar puntos únicos de fallo en la red privada.
