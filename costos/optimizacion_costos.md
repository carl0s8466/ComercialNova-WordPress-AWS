# Plan de Optimización de Costos y FinOps

Se proponen tres acciones de optimización concretas comparando el escenario inicial frente al optimizado:
1. **Right-sizing en Desarrollo:** Escalar hacia abajo las instancias a clases `t3.micro` y `db.t3.micro`, reduciendo drásticamente la facturación base de cómputo.
2. **Políticas de Ciclo de Vida en S3:** Mover los archivos y respaldos antiguos a la clase *S3 Standard-Infrequent Access (S3 IA)* después de 30 días, abaratando el almacenamiento histórico en un 50%.
3. **Horarios de Apagado Automatizado:** Configurar ventanas de apagado automáticas para detener las instancias de pruebas por las noches y fines de semana, disminuyendo un 70% las horas operativas facturables.
* **Costo Optimizado Proyectado:** **~$53.52 USD** (Ahorro global estimado del 59.4%).
