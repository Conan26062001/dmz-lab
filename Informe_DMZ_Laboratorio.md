# Informe de configuraciÃ³n de DMZ con Cisco Packet Tracer


### 1. Objetivo del laboratorio

> Explica brevemente quÃ© se buscaba lograr con este laboratorio.

**Ejemplo:**  
Configurar una DMZ segura usando un router Cisco ISR, aplicando NAT y ACLs para controlar el trÃ¡fico entre LAN, DMZ y red externa.

---

### 2. TopologÃ­a implementada

> Describe la red. Puedes incluir una imagen si el software lo permite (captura de Packet Tracer).

- Cantidad de redes: __________
- Dispositivos usados: __________
- Breve descripciÃ³n de la funciÃ³n de cada zona (LAN, DMZ, Externa).



### 3. Plan de direccionamiento IP

Completa la tabla con las IPs asignadas (puedes copiarla del enunciado si no cambiÃ³).

| Dispositivo             | IP              | MÃ¡scara           | Gateway           |
|-------------------------|------------------|-------------------|-------------------|
| PC_Internal             |                  |                   |                   |
| Server_DMZ              |                  |                   |                   |
| PC_External             |                  |                   |                   |
| Router_FW Gi0/0 (LAN)   |                  |                   |                   |
| Router_FW Gi0/1 (DMZ)   |                  |                   |                   |
| Router_FW Gi0/2 (Ext)   |                  |                   |                   |


### 4. ConfiguraciÃ³n aplicada (resumen)

> Resume los comandos o pasos mÃ¡s relevantes que ejecutaste. Usa texto + fragmentos de cÃ³digo cuando sea necesario.

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

> Â¿QuÃ© aprendiste con este ejercicio? Â¿QuÃ© mejorarÃ­as?

**Ejemplo:**
AprendÃ­ a aplicar NAT y ACLs en un entorno simulado. Recomiendo verificar conectividad bÃ¡sica antes de aplicar reglas de firewall, ya que un error en la IP puede bloquear todo.


### 7. Capturas de evidencia

> Adjunta aquÃ­ (o en un PDF anexo) las capturas solicitadas: pings, navegador, comandos `show`, etc.
