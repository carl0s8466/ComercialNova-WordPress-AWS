# Configuración de Alertas y Observabilidad (CloudWatch)

* **Dashboard de Métricas:** Se implementó un panel centralizado que rastrea la utilización de CPU (`CPUUtilization`) de la capa de cómputo EC2 y la disponibilidad de almacenamiento de la base de datos RDS.
* **Alarma Operacional:** Se configuró la alarma estática `Alerta-CPU-Alta-EC2` sobre la métrica de consumo de CPU. El umbral crítico se definió en un `>= 80%` de uso continuo, lo que activará un estado de alerta en el panel de control ante posibles sobrecargas del portal web.
