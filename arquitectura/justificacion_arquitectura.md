# Justificación de la Arquitectura Cloud - Comercial Nova

Nuestra infraestructura implementa una arquitectura desacoplada de tres capas distribuida en dos Zonas de Disponibilidad (us-east-1a y us-east-1b) para cumplir con los requisitos de alta disponibilidad y seguridad exigidos.

* **Segmentación de Red:** La VPC separa estrictamente los componentes. Las subredes públicas albergan el Application Load Balancer (ALB), único punto de contacto con el exterior. Las subredes privadas contienen el cómputo (EC2) y la base de datos (RDS), eliminando su exposición directa a internet.
* **Tolerancia a Fallos (HA):** Al distribuir las instancias EC2 en diferentes AZs detrás de un ALB, si un centro de datos físico de AWS falla, el tráfico se redirige automáticamente al nodo activo en la otra zona en cuestión de segundos, garantizando un portal corporativo siempre disponible.
