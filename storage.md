# Propuesta Técnica de Almacenamiento y Redundancia
**Cliente:** InnovaCloud Solutions  
**Documento:** `storage.md`  
**Autor:** Kevin Alexander Ortez Oliva 
**Sistema Operativo Base:** Ubuntu Server LTS  

---

## 1. Justificación del Problema: Criticidad de los Fallos de Disco

En el modelo de negocio de **InnovaCloud Solutions**, la disponibilidad continua, la integridad y la confidencialidad de los datos representan el activo operativo fundamental 
para empresas de este tipo. La presencia de discos individuales sin redundancia introduce un **Punto Único de Fallo (SPOF - Single Point of Failure)** que compromete severamente la viabilidad de la empresa por las siguientes razones:

1. **Interrupción de la Continuidad del Negocio y Downtime:**
   Un fallo mecánico o lógico en el disco del servidor principal detiene de forma abrupta los servicios y aplicaciones alojadas. Para una empresa de servicios en la nube, cada minuto de inactividad se traduce en imposibilidad de operación interna y parálisis de los servicios de los clientes conllevando grandes perdidas economicas.

2. **Incumplimiento de Acuerdos de Nivel de Servicio (SLAs):**
   InnovaCloud Solutions está sujeta a contratos de servicio con compromisos de disponibilidad (por ejemplo, 99.9% Tiempo Funcionando "upTime"). La pérdida de servicio o degradación prolongada por reemplazo y reinstalación manual acarrea penalizaciones económicas, disputas legales y rescisión de contratos.

3. **Pérdida y Corrupción Irreversible de Datos Transaccionales:**
   Sin una arquitectura de almacenamiento tolerante a fallos, los datos generados entre el último respaldo periódico (backup) y el momento del fallo se pierden permanentemente (violación del objetivo de punto de recuperación o *RPO*).

4. **Daño Reputacional:**
   En el sector tecnológico y de infraestructura cloud, la confianza del cliente es crítica. La pérdida de datos o la inestabilidad recurrente genera fuga de clientes (*churn*) y deterioro irreversible de la marca en el mercado.

---

## 2. Propuesta Técnica: Implementación de Arreglo Redundante

### 2.1. Selección del Nivel RAID: **RAID 5** (con alternativa RAID 10 para I/O intensivo)
Para el servidor principal de InnovaCloud Solutions, se propone la implementación de **RAID 5 por software** mediante la utilidad nativa de Linux `mdadm`.

* **Justificación de la elección:**
  * **Tolerancia a fallos:** Soporta el fallo completo de 1 disco sin pérdida de información ni interrupción del servicio, reconstruyendo los datos en tiempo real mediante bloques de paridad distribuidos.
  * **Eficiencia de almacenamiento:** A diferencia de RAID 1 (que sacrifica el 50% del almacenamiento bruto), RAID 5 optimiza el costo por gigabyte aprovechando el espacio de $N - 1$ discos (por ejemplo, con 3 discos de 1 TB se obtienen 2 TB útiles con redundancia activa).
  * **Rendimiento de lectura:** Proporciona un incremento sustancial en la velocidad de lectura concurrente al distribuir las operaciones entre múltiples unidades físicas.
  * *(Nota: En caso de cargas de trabajo de bases de datos altamente transaccionales con escrituras intensivas, se recomienda escalar a **RAID 10**).*

--------------------------------------------------------------------------------------------