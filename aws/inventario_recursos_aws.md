# Inventario de Recursos Desplegados en AWS Academy

* **VPC:** `project-vpc` (CIDR 10.0.0.0/16) con 2 subredes públicas y 2 subredes privadas.
* **Cómputo:** 2 Instancias EC2 `WordPress-Web` ejecutando Amazon Linux 2023.
* **Base de Datos:** 1 Instancia RDS MySQL `wordpress-db` (Clase db.t3.micro).
* **Almacenamiento:** 1 Bucket Amazon S3 seguro con versionado activo para medios.
* **Redundancia:** 1 Application Load Balancer `wordpress-alb` y 1 Auto Scaling Group.
