# Informe de configuración de DMZ con Cisco Packet Tracer


### 1. Objetivo del laboratorio

El objetivo de la práctica es configurar direccionamiento IP en una topología compleja, aplicar NAT estático para exponer servicios internos de forma segura y diseñar y aplicar ACLs para controlar el flujo de tráfico y asegurar la red. Todo esto será necesario para crear una Zona Desmilitarizada (DMZ) segura.


### 2. Topologí­a implementada

> Describe la red. Puedes incluir una imagen si el software lo permite (captura de Packet Tracer).

<img width="1837" height="999" alt="image" src="https://github.com/user-attachments/assets/38095f73-6c2e-4663-83db-87e7dbebb2cd" />

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


### 3. Plan de direccionamiento IP

Completa la tabla con las IPs asignadas (puedes copiarla del enunciado si no cambia).

| Dispositivo             | IP               | Máscara           | Gateway           |
|-------------------------|------------------|-------------------|-------------------|
| PC_Internal             |  192.168.1.10    |   255.255.255.0   |   192.168.1.1     |
| Server_DMZ              |  192.168.2.10    |   255.255.255.0   |   192.168.2.1     |
| PC_External             |  192.168.3.10    |   255.255.255.0   |   192.168.3.1     |
| Router_FW Gi0/0 (LAN)   |  192.168.1.1     |   255.255.255.0   |                   |
| Router_FW Gi0/1 (DMZ)   |  192.168.2.1     |   255.255.255.0   |                   |
| Router_FW Gi0/2 (Ext)   |  192.168.3.1     |   255.255.255.0   |                   |


### 4. Configuración aplicada (resumen)

> Resume los comandos o pasos más relevantes que ejecutaste. Usa texto + fragmentos de código cuando sea necesario.

- Interfaces configuradas con `ip address y la máscara de red`

  <img width="1059" height="677" alt="image" src="https://github.com/user-attachments/assets/76502c3c-4935-4121-934a-750e45f71821" />

- NAT:
```bash
ip nat inside source static 192.168.2.10 192.168.3.1
# Este comando hace que desde la red externa (Internet), el servidor (192.168.2.10) será accesible a través de la IP pública 192.168.3.1.
     <img width="1167" height="509" alt="image" src="https://github.com/user-attachments/assets/446a60dd-3b1c-4864-bef0-aa08c59545a0" />

```
- ACLs:
```bash

Extended IP access list 101
    10 permit tcp any host 192.168.3.1 eq 80
    20 deny ip any any
# Esta lista solo permite tráfico HTTP al puerto 80 desde cualquier origen hacia la IP pública del servidor web DMZ.
# Esta ACL debe aplicarse a la interfaz GigabitEthernet0/2 (WAN/red externa) en sentido inbound

Extended IP access list 102
    10 permit tcp 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255 established
    20 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
    30 permit ip any any
# Esta lista deniega cualquier intento de comunicación que se origine desde la red DMZ (192.168.2.0/24) y se dirija hacia la red LAN Interna (192.168.1.0/24).
# Esta ACL debe aplicarse a la interfaz GigabitEthernet0/1 (DMZ) en sentido inbound
```



### 5. Verificaciones realizadas

> Describe las pruebas y su resultado. Incluye capturas o salidas de comandos si se puede.

Antes de configurar el NAT Estático en el Router (Router_FW) hay que verificar la conexión de la siguiente manera:

Hay que comprobar que cada dispositivo (endpoint) pueda alcanzar a su respectivo gateway.
Para eso se usa la herramienta "Commando Prompt" que se encuentra en el apartado de Desktop en cada dispositivo.

Desde PC_Internal:
     ping 192.168.1.1 (Ping a su Gateway)
          Resultado esperado: Replies
          <img width="971" height="721" alt="image" src="https://github.com/user-attachments/assets/a8792d69-aa59-4bb2-bd23-2ba7b90f3ba5" />

Desde Server-PT Web_DMZ :
     ping 192.168.2.1 (Ping a su Gateway)
          Resultado esperado: Replies 
          <img width="981" height="763" alt="image" src="https://github.com/user-attachments/assets/5d56a271-32d2-444d-9d3e-4fc281fb9803" />

Desde PC_External :
     ping 192.168.3.1 (Ping a su Gateway)
          Resultado esperado: Replies 
          <img width="969" height="721" alt="image" src="https://github.com/user-attachments/assets/182a3dc6-479e-4f86-8f1e-8749208acc80" />

Como último paso hay que realizar unas pruebas para verificar que la DMZ es segura y funcional.
Las pruebas son las siguientes:

Desde PC_External via Web Browser (se encuentra en el apartado de Desktop en el dispositivo):
     Accede al servidor web DMZ escribiendo 192.168.3.1 en el buscador.
          Resultado esperado: La página web debe cargar.
     <img width="1255" height="805" alt="image" src="https://github.com/user-attachments/assets/444dc21f-7b03-44fe-ac4a-c8385001eb48" />

Desde PC_External via Command Prompt (se encuentra en el apartado de Desktop en el dispositivo):
     ping 192.168.3.1 (Ping a la interfaz WAN/IP pública del servidor). 
          Resultado esperado: Request timed out (Debe FALLAR si tu ACL externa bloquea ICMP).
     <img width="991" height="667" alt="image" src="https://github.com/user-attachments/assets/dd213403-4e07-4ad1-90d3-5ffdd180248e" />

Desde PC_Internal via Web Browser (se encuentra en el apartado de Desktop en el dispositivo):
     Accede al servidor web DMZ escribiendo 192.168.2.10 en el buscador.
          Resultado esperado: La página web debe cargar.
     <img width="1239" height="688" alt="image" src="https://github.com/user-attachments/assets/73d40d3c-2e3f-467f-86ee-ca223e80d083" />

Desde Server-PT Web_DMZ via Command Prompt (se encuentra en el apartado de Desktop en el dispositivo):
     ping 192.168.1.10 (Ping a PC_Internal). 
          Resultado esperado: Request timed out (Debe FALLAR - ¡Esto es seguridad crucial!).
     <img width="1005" height="654" alt="image" src="https://github.com/user-attachments/assets/6ec5370e-ff83-4334-ab3c-d418ed3fd366" />

### 6. Conclusiones y recomendaciones

> ¿Qué aprendiste con este ejercicio? ¿Qué mejorarías?

He aprendido a crear NAT y ACLs extendidas en un entorno simulado, a verificar la efectividad de las ACLs.
He aprendido a verificar la conectividad básica y a fijarse en las IPs ya que un número cambia mucho.


