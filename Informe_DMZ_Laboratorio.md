# Informe de configuración de DMZ con Cisco Packet Tracer


### 1. Objetivo del laboratorio

El objetivo de la práctica es configurar direccionamiento IP en una topología compleja, aplicar NAT estático para exponer servicios internos de forma segura y diseñar y aplicar ACLs para controlar el flujo de tráfico y asegurar la red. Todo esto será necesario para crear una Zona Desmilitarizada (DMZ) segura.

---

### 2. Topologí­a implementada

> Describe la red. Puedes incluir una imagen si el software lo permite (captura de Packet Tracer).

- Cantidad de redes:
     Hay 3 redes que son las siguientes:
        GigabitEthernet0/0          GigabitEthernet0/1          GigabitEthernet0/2
- Dispositivos usados:
    Hay un router Cisco ISR con el nombre de Router_FW
    Hay tres switches Cisco 2960, uno para cada red
    Hay dos ordenadores, el PC_Internal y el PC_External
    Hay un servidor llamado Server-PT Web_DMZ
  
- Breve descripción de la función de cada zona (LAN, DMZ, Externa).
  La red interna (LAN) conecta los equipos de la empresa con el servidor de la empresa
  La DMZ aloja aislado al servidor de la empresa
  La red externa conecta a los clientes con el servidor de la empresa de forma segura

<img width="1837" height="999" alt="image" src="https://github.com/user-attachments/assets/38095f73-6c2e-4663-83db-87e7dbebb2cd" />


### 3. Plan de direccionamiento IP

Completa la tabla con las IPs asignadas (puedes copiarla del enunciado si no cambia).

| Dispositivo             | IP               | Máscara           | Gateway           |
|-------------------------|------------------|-------------------|-------------------|
| PC_Internal             |  192.168.1.10    |   255.255.255.0   |   192.168.1.1     |
| Server_DMZ              |  192.168.2.10    |   255.255.255.0   |   192.168.2.1     |
| PC_External             |  192.168.3.10    |   255.255.255.0   |   192.168.3.1     |
| Router_FW Gi0/0 (LAN)   |                  |                   |                   |
| Router_FW Gi0/1 (DMZ)   |                  |                   |                   |
| Router_FW Gi0/2 (Ext)   |                  |                   |                   |


### 4. Configuración aplicada (resumen)

> Resume los comandos o pasos más relevantes que ejecutaste. Usa texto + fragmentos de código cuando sea necesario.

- Interfaces configuradas con `ip address`
- NAT:
```bash
ip nat inside source static 192.168.2.10 192.168.3.1
```
- ACLs:
```bash
access-list 101 permit tcp any host 192.168.3.1 eq 80
access-list 100 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
```



### 5. Verificaciones realizadas

> Describe las pruebas y su resultado. Incluye capturas o salidas de comandos si se puede.

- `ping` desde PC_Internal al router: âœ…
- Acceso web desde PC_External: âœ…
- Bloqueo de acceso desde DMZ a LAN: âœ…


### 6. Conclusiones y recomendaciones

> ¿Qué aprendiste con este ejercicio? ¿Qué mejorarías?

**Ejemplo:**
AprendÃ­ a aplicar NAT y ACLs en un entorno simulado. Recomiendo verificar conectividad bÃ¡sica antes de aplicar reglas de firewall, ya que un error en la IP puede bloquear todo.


### 7. Capturas de evidencia

> Adjunta aquÃ­ (o en un PDF anexo) las capturas solicitadas: pings, navegador, comandos `show`, etc.
