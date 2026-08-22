# Consulting Group - Proyecto InnovaCloud Solutions

Bienvenidos al repositorio oficial de nuestra firma de consultoría tecnológica. Este documento presenta la solución integral diseñada para estabilizar, asegurar y optimizar la infraestructura de servidores de nuestro cliente, **InnovaCloud Solutions**, operando sobre entornos **Ubuntu Server LTS**.

## Síntesis de la Solución Integral

Tras realizar una auditoría exhaustiva a la infraestructura del cliente, identificamos debilidades críticas que amenazaban la continuidad del negocio, la eficiencia operativa y la seguridad. Nuestra intervención resuelve estas problemáticas a través de los siguientes cuatro pilares estratégicos:

1. **Almacenamiento y Tolerancia a Fallos (`storage.md`):**
   Se mitigó el Punto Único de Fallo (SPOF) en los servidores principales mediante la implementación de un arreglo **RAID 5 por software** utilizando `mdadm`. Esta arquitectura garantiza la redundancia de los datos frente a fallos mecánicos de disco, asegurando el cumplimiento de los Acuerdos de Nivel de Servicio (SLAs) sin sacrificar la eficiencia del espacio de almacenamiento.

2. **Gestión de Infraestructura de Software (`packages.md`):**
   Para erradicar la inconsistencia de versiones ("Dependency Hell") y la saturación injustificada del ancho de banda WAN, implementamos un **Repositorio Espejo (Mirror) Local** y estandarizamos la administración mediante el gestor **APT**. Esto permite despliegues centralizados a velocidad de red LAN, garantizando entornos homogéneos y seguros.

3. **Arquitectura y Enrutamiento de Red de Desarrollo (`networking.md`):**
   Se eliminó el aislamiento de los entornos de prueba provocado por la configuración NAT por defecto. La solución consistió en migrar las interfaces a modo **Adaptador Puente (Bridged)** y orquestar el direccionamiento IP estático a través de **Netplan**. Esto integra directamente las máquinas virtuales a la red corporativa, permitiendo una colaboración fluida entre los desarrolladores.

4. **Auditoría, Diagnóstico y Seguridad (`diagnostics.md`):**
   Se redujeron los tiempos de *troubleshooting* (Downtime) y se cerraron brechas de seguridad estableciendo un protocolo de auditoría estandarizado. Mediante el despliegue de herramientas nativas avanzadas (`ip`, `ping`, `traceroute`, `systemctl`, `ss`, `nmap`), el equipo de InnovaCloud ahora posee control total para monitorear interfaces activas, disponibilidad de *daemons* y la superficie de ataque en los puertos del servidor.

---

## Equipo Consultor (Autores y Roles)

A continuación, se detalla el equipo de especialistas responsable de la consultoría y el diseño de la solución técnica:

| Nombre del Consultor | Carnet | Área de Especialidad / Rol | Documentación |
| :--- | :--- | :--- | :--- |
| **Kevin Alexander Ortez Oliva** | *oo231494* | Consultor de Almacenamiento y Redundancia | [`storage.md`](./storage.md) |
| **William Edmundo Montano Molina** | *mm230517* | Consultor de Infraestructura y Gestión de Software | [`packages.md`](./packages.md) |
| **David Antonio Gavidia Garcia** | *gg231267* | Consultor de Arquitectura de Redes y Conectividad | [`networking.md`](./networking.md) |
| **Anderson Alessandro Moya Alegria** | *ma230398* | Consultor de Verificación, Seguridad y Diagnóstico | [`diagnostics.md`](./diagnostics.md) |


---

##  Entregables y Enlaces del Proyecto

*   **Repositorio Oficial (GitHub):** [https://github.com/KenOO2/Desafio_WilliamSquad_Pedido06]
*   **Video de la Defensa Técnica:** [https://drive.google.com/drive/folders/18OiW_RKrxZxC-vfRZTjoMYkPmazRcRQE?usp=sharing]
