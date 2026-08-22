Cliente: InnovaCloud Solutions

Documento: diagnostics.md

Autor: Anderson Alessandro Moya Alegria


1. Identificación del Problema: Ausencia de Estandarización en Diagnósticos
Actualmente, InnovaCloud Solutions carece de un procedimiento uniforme para auditar la operatividad de su infraestructura. Esta falta de estandarización impacta negativamente al negocio en tres áreas críticas:

Aumento del Downtime (Tiempo de inactividad): Sin protocolos definidos, el troubleshooting (resolución de problemas) depende del método individual de cada administrador. Esto retrasa el diagnóstico y prolonga la caída de los sistemas.

Vulnerabilidades de Seguridad: La omisión de auditorías periódicas sobre las conexiones de red expande la superficie de ataque, dejando puertos expuestos que podrían ser explotados para accesos no autorizados.

Consumo Ineficiente de Recursos: Al no monitorear continuamente el entorno, servicios inactivos o mal configurados (procesos zombis o colgados) consumen CPU y memoria de los servidores de forma innecesaria.

2. Propuesta de Solución: Protocolo Estandarizado de Auditoría
Para mitigar estos riesgos, nuestra firma de consultoría propone implementar un marco de trabajo estandarizado basado en herramientas nativas de Linux. Todo el equipo técnico deberá utilizarlas para garantizar el control de la infraestructura:

A. Validación de Enlaces e Interfaces Físicas/Lógicas
Propósito técnico/operativo: Auditar rápidamente la capa de red y enlace. Permite confirmar que las tarjetas de red (NICs) se encuentran en estado activo (UP) y con el direccionamiento IP correcto antes de escalar hacia problemas más complejos.

Entorno de ejecución: Directamente en la terminal del servidor Linux virtualizado o la máquina física que presenta fallas de conexión local.

Ejemplos de ejecución:

# Verificar el direccionamiento IP y estado de todas las interfaces
ip a

# Validar únicamente el estado físico y la capa de enlace (direcciones MAC)
ip link show
B. Pruebas de Enrutamiento y Conectividad (Extremo a Extremo)
Propósito técnico/operativo: Evaluar el envío de paquetes ICMP para medir la latencia y trazar la ruta exacta de los datos. Es esencial para aislar fallas, detectando si la interrupción ocurre en el servidor local, en el enrutamiento o en el destino final.

Entorno de ejecución: Desde el servidor de origen hacia la IP del servidor de destino.

Ejemplos de ejecución:

# Enviar 4 paquetes de prueba a un servidor externo (ej. DNS de Google)
ping -c 4 8.8.8.8

# Trazar la ruta de saltos hacia la red corporativa o un host específico
traceroute 192.168.1.50
C. Monitoreo de Alta Disponibilidad de Servicios (Daemons)
Propósito técnico/operativo: Garantizar que los procesos críticos (como bases de datos o servidores web) estén operando sin interrupciones. Permite auditar el estado en tiempo real y extraer los logs exactos para diagnosticar errores sin detener el servicio.

Entorno de ejecución: Exclusivamente en el servidor de destino que aloja el servicio específico.

Ejemplos de ejecución:

Bash
# Verificar si el servicio web Apache está activo y corriendo
systemctl status apache2

# Extraer las últimas 50 líneas de registro de errores de un servicio específico
journalctl -u nginx -n 50 --no-pager
D. Análisis de Superficie de Ataque (Puertos Abiertos)
Propósito técnico/operativo: Mapear los sockets de conexión TCP/UDP. Permite al equipo identificar exactamente qué puertos están en estado de escucha (listening) y qué proceso los ejecuta, facilitando el cierre preventivo de brechas de seguridad.

Entorno de ejecución: Directamente en el servidor perimetral o cualquier servidor interno para auditar sus propias políticas de puertos expuestos.

Ejemplos de ejecución:

# Listar todos los puertos TCP/UDP en escucha con sus respectivos procesos (PID)
ss -tulpn

# Escanear el propio servidor (localhost) para detectar versiones de servicios corriendo
nmap -sV localhost