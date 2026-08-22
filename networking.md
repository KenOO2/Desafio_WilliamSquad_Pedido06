# Propuesta Técnica de Configuración de Red

**Cliente:** InnovaCloud Solutions
**Documento:** `networking.md`
**Autor:** David Antonio Gavidia Garcia
**Sistema Operativo Base:** Ubuntu Server LTS

---

## 1. Justificación del Problema: Limitaciones del Modo NAT

Al evaluar la infraestructura de **InnovaCloud Solutions**, identificamos que el uso del modo NAT (la configuración por defecto en VirtualBox) en las máquinas virtuales limita significativamente el entorno de desarrollo colaborativo. Este modo aísla la máquina de la red principal, generando los siguientes inconvenientes operativos:

1. **Aislamiento de los servicios:** Aunque la máquina virtual cuenta con acceso a internet, no es visible para el resto de los equipos en la red corporativa. Esto impide que otros desarrolladores puedan consumir directamente las bases de datos o servicios en fase de prueba alojados en ella.
2. **Sobrecarga en la configuración:** Para permitir el acceso externo hacia la máquina virtual en modo NAT, sería necesario configurar reglas de redirección de puertos (Port Forwarding) de manera constante. Esto resulta ineficiente para el equipo y es muy susceptible a errores.
3. **Retraso en las integraciones:** La falta de comunicación directa y fluida entre los entornos de prueba dificulta el trabajo en equipo y retrasa las entregas.

---

## 2. Propuesta Técnica: Adaptador Puente y Netplan

### 2.1. Selección del Modo de Red: Adaptador Puente (Bridged Adapter)

Para integrar las máquinas virtuales al entorno de la empresa, evaluamos las opciones de VirtualBox. Descartamos el modo "Red Interna" porque, aunque es seguro, deja a la máquina sin salida a internet y sin conexión con el host físico. Por lo tanto, la propuesta es migrar al modo **Adaptador Puente**.

Esta opción conecta la interfaz de la máquina virtual directamente a la red física del anfitrión. De esta manera, la máquina virtual solicita y obtiene una dirección IP directamente del router de la empresa, funcionando en la práctica como un equipo físico más dentro de la oficina. Esto resuelve de inmediato los problemas de acceso entre los desarrolladores.

### 2.2. Configuración de IP Estática mediante Netplan

Para asegurar que los servicios de desarrollo mantengan una ubicación constante y no dependan de una asignación dinámica (DHCP), es necesario fijar una IP estática. A continuación se detallan los pasos de configuración utilizando Netplan:

**Paso 1:** Identificar el nombre de la interfaz de red (comúnmente `enp0s3`).
```bash
ip a
```

**Paso 2:** Editar el archivo de configuración de red YAML con privilegios de administrador.
```bash
sudo nano /etc/netplan/01-network-manager-all.yaml
```

**Paso 3:** Establecer la configuración. A modo de ejemplo, asumiendo que la red utiliza el segmento `192.168.1.0/24`, se asignará la IP `.50` a la máquina virtual:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 192.168.1.50/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

**Paso 4:** Guardar el archivo, aplicar los cambios en el sistema y verificar la conectividad externa.
```bash
sudo netplan apply
ping -c 4 8.8.8.8
```
