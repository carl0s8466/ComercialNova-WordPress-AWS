# Matriz de Control de Accesos y Reglas de Firewall

Se aplica rigurosamente el principio de mínimo privilegio restringiendo el tráfico mediante Security Groups:

1. **Web-SG (Capa de Aplicación):** Permite tráfico entrante HTTP (puerto 80) y HTTPS (puerto 443) desde cualquier origen (`0.0.0.0/0`). El acceso administrativo se restringe al protocolo SSH (puerto 22) mapeado exclusivamente por AWS Systems Manager Session Manager.
2. **DB-SG (Capa de Datos):** Permite tráfico entrante en el puerto MySQL (3306) **únicamente** si el origen proviene de un recurso que posea el grupo de seguridad `Web-SG`, aislando la base de datos de cualquier petición externa directa.
