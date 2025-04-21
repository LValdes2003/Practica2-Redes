# Practica2-Redes
## 1 Modelado de la Comunicación (OSI/TCP‑IP):

**El objetivo es permitir:**

1. Transferencia segura de archivos grandes (como documentos de investigación o software educativo).

2. Streaming fluido de contenido multimedia (como conferencias grabadas y material interactivo). Ambos deben funcionar correctamente en una red heterogénea con cableado Ethernet y conexiones WiFi.

Aunque OSI y TCP/IP son modelos diferentes, se complementan. El modelo TCP/IP es el más usado en la práctica, pero el modelo OSI permite analizar mejor cada capa. En este caso, podemos ver cómo las funciones de OSI están contenidas dentro de las capas de TCP/IP, lo cual ayuda a entender la transmisión completa.

## Transmisión de Archivos

- TCP/IP (Capa de Aplicación) → Utiliza protocolos como FTP/SFTP.

    -  En OSI, incluye las capas 5 (Sesión), 6 (Presentación) y 7 (Aplicación).

    - Se encarga de establecer sesiones seguras y cifrar los datos.

- Capa de Transporte → Usa TCP, garantiza que el archivo llegue completo y ordenado.

- Capa de Internet (Red) → Protocolo IP para enrutar los paquetes.

- Capa de Acceso a Red → Ethernet/WiFi y dispositivos de red físicos.

## Transmisión Multimedia (Streaming)

- TCP/IP (Capa de Aplicación) → Utiliza RTSP, HTTP Live Streaming (HLS), o DASH.

    - En OSI, se encarga del control de sesiones y codificación de contenido.

- Capa de Transporte → UDP (menos confiable pero más rápido).

- Capa de Internet → IP, igual que en la transmisión de archivos.

- Capa de Acceso a Red → Ethernet/WiFi.

Ambos modelos trabajan en paralelo: TCP/IP opera en la práctica, mientras que OSI ayuda a entender qué ocurre en cada fase del proceso de comunicación.

## Paso 2: Capa Física:

El rendimiento de una red es dependiente en las características de su capa fisica, que realiza la transmisión de datos a traves de los ditintos tipos de enlaces, si cableados o inalámbricos, y también por su tipo de modulacion

### Cálculo de la Tasa de Transmisión 

Para calcular la tasa de transmision de los enlaces critcos de la red se necesita el ancho de banda y el SNR.  

Se utiliza la fórmula de Shannon:  

$$Donde: C = B * log_2(1+SNR)$$  

Ancho de banda (B)  

SNR: relación señal a ruido determinada.   
La relación señal a ruido se mide en dB en un ancho de banda es:    

$$𝑆𝑁𝑅 = 10 𝑙𝑜𝑔_{10}(𝑆𝑁𝑅) = 10^{\frac{SNR}{10}} [dB]$$  

Dado que es necesario el streaming y la tranferencia de archivos grandes se recomiendo un SNR de al menos 30.  
$$SNR lineal= 10^{\frac{30}{10}} = 10^3 = 1000$$

- *Ahora para los distintos casos*

1. *Cobre*  
    Cable de cobre categoria 6 con ancho de banda de 250 Mhz. Entre switches y acces points.  

    $$C = 2,5 * 10^8 * log_2(1+1000) = 2,5 * 10^8 * log_2(1001) = 2,5 * 10^8 * 9.967 = 2.49 * 10^9bps = 2.49 Gbps$$


    Dado que los access points limitan la transmision a 100 Mbps al switch debido a la conexion del puerto todo esos calculos antes son teoricos y la velocidad verdadera es 100Mbps

2. *Resto de red cableada*  
    Cable  de un Ghz de ancho de banda para mayor trafico.

    $C = 1* 10^9 * log_2(1001) =  1 * 10^9 * 9.967 = 9,96710^9 bps = 9.97 Gbps$

### Tipo de modulacion

Por otro lado, la técnica de modulación empleada también influye directamente en la eficiencia del uso del ancho de banda y en la robustez frente al ruido. 
- Para los enlaces cableados como Ethernet, es recomendable utilizar esquemas de modulación como 16-QAM o superiores, que ofrecen una buena relación entre eficiencia espectral y confiabilidad.  
- Para redes WiFi, se emplea la configuración 64-QAM que tiende mas hacia el rendimiento que la robustez mientras que mantiene unos requerimientos racionales y no muy altos. Dado que es un canal inestable se emplearian  un esquema ODFM, que divide el canal en multiples subportadoras y ofrece una gran robustez ante interferencias.


---

## Paso 3: Capa de Red

### Esquema del direccionamiento

La red principal es la 192.168.20.0:
- *Rango de Hosts:* 192.168.20.2 – 192.168.20.254
- *Default gateway:* 192.168.20.1
- *Broadcast:* 192.168.20.255
- *Total de Hosts Útiles:* 254

Aparte la red se divide en multiple vlans cada una con su propia IP, gateway y broadcast. En concreto 3: la 100, la 200, la 300

| VLAN     | Descripción   | Red IP             | Gateway            | Broadcast            |
|----------|---------------|--------------------|--------------------|----------------------|
| 100      | Recepción     | 192.168.100.0/24   | 192.168.100.1      | 192.168.100.255      |
| 200      | Planta 1      | 192.168.200.0/24   | 192.168.200.1      | 192.168.200.255      |
| 300      | Planta 2      | 192.168.30.0/24    | 192.168.30.1       | 192.168.30.255       |
| 400      | Planta 3      | 192.168.60.0/24    | 192.168.60.1       | 192.168.60.255       |
| 500      | Biblioteca    | 192.168.69.0/24    | 192.168.69.1       | 192.168.69.255       |
| 600      | Laboratorio   | 192.168.90.0/24    | 192.168.90.1       | 192.168.90.255       |

Cada VLAN tiene su propia subred /24 (máscara de subred 255.255.255.0), lo cual permite hasta 254 hosts por segmento, ideal para entornos académicos de tamaño medio.

#### VLAN 100 – 192.168.100.0/24

- *Rango de Hosts:* 192.168.100.2 – 192.168.100.254
- *DDefault gateway:* 192.168.100.1
- *Broadcast:* 192.168.100.255
- *Total de Hosts Útiles:* 254

#### VLAN 200 – 192.168.200.0/24

- *Rango de Hosts:* 192.168.200.2 – 192.168.200.254
- *Default gateway:* 192.168.200.1
- *Broadcast:* 192.168.200.255
- *Total de Hosts Útiles:* 254

#### VLAN 300 – 192.168.30.0/24

- *Rango de Hosts:* 192.168.30.2 – 192.168.30.254
- *Default gateway:* 192.168.30.1
- *Broadcast:* 192.168.30.255
- *Total de Hosts Útiles:* 254

#### VLAN 400 – 192.168.60.0/24

- **Rango de Hosts:** 192.168.60.2 – 192.168.60.254
- *Default gateway:* 192.168.60.1
- *Broadcast:* 192.168.60.255
- *Total de Hosts Útiles:* 254

#### VLAN 500 – 192.168.69.0/24

- **Rango de Hosts:** 192.168.69.2 – 192.168.69.254
- *Default gateway:* 192.168.69.1
- *Broadcast:* 192.168.69.255
- *Total de Hosts Útiles:* 254

#### VLAN 600 – 192.168.90.0/24

- *Rango de Hosts:* 192.168.90.2 – 192.168.90.254
- *Default gateway:* 192.168.90.1
- *Broadcast:* 192.168.90.255
- *Total de Hosts Útiles:* 254


---
## Paso 4: Capa de Transporte – Selección de Protocolos

### Decisión de Protocolos:
En el diseño de una red académica, la elección del protocolo de transporte adecuado es crucial para asegurar un rendimiento eficiente y una experiencia fluida para los usuarios.
-  Para la transferencia de archivos como documentos, proyectos o recursos digitales, se recomienda el uso del protocolo *TCP (Transmission Control Protocol)*, ya que ofrece características esenciales como el control de flujo, la confirmación de entrega, corrección de errores y retransmisión de paquetes perdidos. Estas funciones garantizan la integridad de los archivos transferidos, aunque pueden introducir una ligera latencia, lo cual es aceptable en este tipo de aplicaciones.

- Por otro lado, para el streaming el protocolo indicado es *UDP (User Datagram Protocol)*. A diferencia de TCP, UDP no requiere confirmaciones ni mantiene el estado de la conexión, lo que reduce considerablemente la latencia. El protocolo se rige por la velocidad y la continuidad del flujo de datos son más importantes que la entrega perfecta de cada paquete. Aunque puede haber pequeñas pérdidas de información, estas no afectan significativamente al total.


### Cálculo de la Ventana:

Para optimizar el rendimiento de la red en la transferencia de archivos mediante el protocolo TCP, se debe calcular el tamaño óptimo de la *ventana de transmisión TCP*. Esta ventana determina cuántos datos pueden enviarse antes de recibir una confirmación del receptor, y su correcta configuración permite aprovechar al máximo el ancho de banda disponible.


Haciendo un ping se reciven los siguientes valores para el RTT:minimum = $31 ms$, maximun = $52 ms$, average = $38 ms$

Convertimos RTT a segundos:

RTT = 38 ms = 0.038 segundos

Aplicamos la fórmula de la ventana:



$$\text{Ventana (bytes)} = \frac{Ancho de Banda (bps) \times RTT (seg)}{8}$$  



$$\text{Ventana (bytes)} = \frac{100,000,000 \times 0.038}{8} = \frac{3,800,000}{8} = 475,000 \text{ bytes}$$

Luego, calculamos cuántos segmentos TCP de tamaño estándar (MSS) se pueden enviar:

$$
\text{Segmentos} = \frac{475,000}{1460} \approx 325.3 \text{ segmentos}
$$

#### Resultado:

El tamaño óptimo de la ventana de transmisión TCP en los enlaces simulados es de aproximadamente **475,000 bytes**, lo que equivale a unos **325 segmentos** de 1460 bytes. Este tamaño garantiza una transmisión eficiente, minimizando esperas por confirmaciones y aprovechando al máximo la capacidad del canal.

## Paso 5: Capa de Aplicación – Servicios y Multiplexación

### Servicios de Transferencia de Archivos (FTP/SFTP)

SFTP (Secure File Transfer Protocol)

- Uso: Transferencia de archivos grandes entre laboratorios, bibliotecas y aulas.

- Seguridad: Cifrado extremo a extremo mediante SSH.

- Puertos: TCP 22.

- Autenticación: Basada en contraseñas o llaves SSH.

- Implementación:

    - Servidor: Instancia de OpenSSH Server o vsftpd con soporte SFTP.

    - Cliente: Aplicaciones como FileZilla, WinSCP o desde terminal (sftp user@host).

    - Multiplexación: Varios usuarios pueden usar el servicio simultáneamente mediante túneles seguros por SSH, cada uno con su sesión independiente.

FTP/FTPS (menos recomendado, pero opcional)
- FTPS (FTP sobre SSL/TLS): Añade seguridad a FTP tradicional.

- Puertos: TCP 21 (control) + rango dinámico (datos).

- Consideración: Más complejo de configurar en firewalls.

### Servicios de Streaming y Contenido Multimedia (HTTP/HTTPS)
HTTP/HTTPS
- Uso: Acceso a conferencias grabadas, contenido interactivo, materiales educativos.

- Seguridad: HTTPS (TLS 1.2+).

- Puertos: HTTP (80), HTTPS (443).

- Implementación:

    - Servidor web: Apache, Nginx o un servidor LMS (Moodle, Canvas).

    - Streaming: Soporte para protocolos como:

        - HLS (HTTP Live Streaming) o DASH

        - Adaptabilidad de calidad en tiempo real (ajuste al ancho de banda)

    - Clientes: Navegadores web, apps móviles, reproductores embebidos.

 Multiplexación
- Un único servidor web puede manejar múltiples solicitudes HTTP/HTTPS simultáneamente.

- Cada cliente establece una sesión individual (usando cookies, tokens, TLS sessions).

- Se usan threads o procesos concurrentes/asíncronos en el servidor para gestionar múltiples clientes a la vez.

### Cifrado de Datos: Simétrico y Asimétrico

Cifrado Simétrico
- Utiliza una clave única compartida entre emisor y receptor.

- Es rápido y eficiente, ideal para grandes volúmenes de datos como archivos o video.

- Uso típico:

    - Una vez establecida la conexión segura (por ejemplo, mediante TLS), se genera una clave simétrica (ej. AES-256).

    - Esta clave se usa para cifrar todos los datos durante la sesión.

**Ejemplo:** Durante una descarga vía SFTP o reproducción de video por HTTPS, los datos se cifran usando una clave simétrica.

Cifrado Asimétrico
- Utiliza un par de claves: una pública y una privada.

- Es más lento, pero permite:

    - El intercambio seguro de claves simétricas.

    - La verificación de identidad mediante certificados digitales.

- Uso típico:

    - Durante el handshake TLS (en conexiones HTTPS o SFTP), el cliente utiliza la clave pública del servidor para enviar de forma segura una clave simétrica.

    - El servidor descifra con su clave privada.

**Ejemplo:** Un cliente se conecta a un servidor web. El servidor responde con su certificado (clave pública). El cliente verifica la firma y establece un canal seguro para enviar la clave simétrica.



## Paso 6: Seguridad – Estrategias y Configuraciones

### Medidas de Seguridad Implementadas

La seguridad de la red académica se ha estructurado en múltiples capas, integrando mecanismos preventivos, defensivos y de segmentación lógica. Las principales medidas son:

#### 1. Conexión mediante VPN para Segmentos Remotos

Se implementa una *VPN (Virtual Private Network)* para permitir conexiones seguras entre usuarios remotos (por ejemplo, docentes trabajando desde casa o campus satélite) y la red académica interna. Esta VPN cifra todo el tráfico entre el cliente y la red local, protegiendo los datos incluso si se transmiten por redes públicas como Internet.

La VPN se ubica *antes del firewall, en el perímetro de la red, lo que permite autenticar al usuario y encapsular el tráfico antes de que ingrese al núcleo interno. Se recomienda el uso de protocolos como **IPSec* o *SSL VPN*, según el soporte de los dispositivos simulados.

#### 2. Firewall Perimetral con ACLs

El firewall se posiciona estratégicamente en la entrada a la red interna, actuando como la primera línea de defensa. Se configura con *ACLs (Access Control Lists)* que filtran el tráfico según:

- Dirección IP de origen y destino
- Puerto de servicio (HTTP, FTP, etc.)
- Protocolo utilizado (TCP, UDP, ICMP)

Esto permite bloquear accesos no autorizados desde el exterior, limitar conexiones innecesarias entre VLANs, y prevenir ataques como escaneo de puertos o denegación de servicio (DoS).

#### 3. Separación por VLANs

La red está segmentada en vlans que añaden medidas de seguridad y encapsulación:

- Aísla el tráfico entre grupos funcionales
- Reduce la superficie de ataque
- Mejora la contención ante incidentes
