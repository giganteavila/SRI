Para identificar qué dispositivo o servicio ha asignado una dirección IP a un dispositivo en tu red local, puedes seguir varios métodos. A continuación, se detallan diferentes enfoques que te ayudarán a determinar la fuente de asignación de IP, ya sea un servidor DHCP dedicado, tu router, o algún otro dispositivo en la red.
1. Verificar la Configuración de Red del Dispositivo

a. En Linux:

Puedes utilizar el comando nmcli o ip para obtener detalles sobre la configuración de red, incluyendo el servidor DHCP que ha asignado la IP.

    Usando nmcli:
    
    bash

nmcli device show enp3s0 | grep DHCP

Este comando mostrará información relacionada con DHCP, incluyendo la dirección del servidor DHCP (DHCP4.OPTION).

Usando ip:

bash

ip addr show enp3s0

Para obtener información más detallada sobre el proceso DHCP, puedes revisar los logs del sistema:

bash

    sudo journalctl -u NetworkManager | grep DHCP

b. En Windows:

    Abre una Ventana de Comandos (CMD) con privilegios de administrador.
    
    Ejecuta el siguiente comando:
    
    cmd
    
    ipconfig /all
    
    Busca la sección de la interfaz de red utilizada (por ejemplo, Ethernet) y localiza la línea "Servidor DHCP". Esta línea mostrará la dirección IP del servidor que ha asignado la dirección IP actual.

c. En macOS:

    Abre la Terminal.
    
    Ejecuta:
    
    bash
    
    ipconfig getpacket en0
    
    Reemplaza en0 con la interfaz de red correspondiente si es necesario.
    
    Busca la línea que indica el "server_identifier", que corresponde al servidor DHCP que ha asignado la IP.

2. Revisar el Router o Servidor DHCP

Si tu red local utiliza un router como servidor DHCP, puedes acceder a la interfaz de administración del router para ver una lista de dispositivos conectados y el servidor DHCP que está asignando las direcciones IP.

Pasos Generales:

    Accede a la interfaz web del router:
        Abre un navegador web.
        Ingresa la dirección IP del router en la barra de direcciones (comúnmente 192.168.1.1, 192.168.0.1, 172.16.0.1, etc.).
        Inicia sesión con tus credenciales de administrador.
    
    Navega a la sección de DHCP:
        Busca una sección etiquetada como "DHCP", "LAN", "Network", o similar.
        Aquí podrás ver una tabla de "Clientes DHCP" o "Dispositivos Conectados" que muestra qué dispositivos han recibido direcciones IP y desde qué servidor.
    
    Identifica el Dispositivo:
        Busca la dirección IP en cuestión y verifica qué dispositivo o dirección MAC está asociado a ella.

Nota: Si tienes múltiples servidores DHCP en la red (por ejemplo, un router y un servidor dedicado), es posible que debas revisar cada uno para identificar la fuente correcta.
3. Utilizar Herramientas de Análisis de Red

**a. Wireshark:

Wireshark es una herramienta poderosa para capturar y analizar tráfico de red. Puedes usarla para interceptar paquetes DHCP y determinar qué servidor está asignando direcciones IP.

Pasos Básicos:

    Instala Wireshark:
        En Linux: sudo apt install wireshark
        En Windows y macOS: Descarga desde Wireshark.org.
    
    Inicia una Captura de Paquetes:
        Abre Wireshark y selecciona la interfaz de red que deseas monitorear (por ejemplo, enp3s0).
    
    Filtra los Paquetes DHCP:
        En la barra de filtros, ingresa dhcp o bootp para filtrar solo los paquetes DHCP.
    
    Analiza los Paquetes:
        Observa los paquetes DHCP Offer y DHCP ACK para identificar la dirección IP del servidor DHCP (Server Identifier).

**b. dhcping:

dhcping es una herramienta de línea de comandos que puede enviar solicitudes DHCP a un servidor específico y verificar si está activo.

Instalación:

    En Debian/Ubuntu:
    
    bash
    
    sudo apt install dhcping

Uso:

bash

dhcping -c <tu_dirección_IP> -s <dirección_IP_del_servidor_DHCP>

Este comando verifica si el servidor DHCP especificado está respondiendo a las solicitudes.
4. Inspeccionar la Tabla de ARP

La Tabla ARP (Address Resolution Protocol) mapea direcciones IP a direcciones MAC. Si conoces la dirección MAC del servidor DHCP, puedes identificarlo en la tabla ARP.

Comando en Linux:

bash

arp -a

Busca la dirección IP asignada y verifica su dirección MAC correspondiente.

Nota: Este método es menos directo, pero puede ser útil si conoces la dirección MAC del servidor DHCP.
5. Consultar los Logs del Servidor DHCP

Si tienes acceso al servidor DHCP (por ejemplo, si estás utilizando un servidor DHCP dedicado en Linux), puedes revisar sus logs para ver qué dispositivos han recibido direcciones IP.

En un servidor DHCP basado en ISC DHCP:

Los logs suelen estar en /var/log/syslog o /var/log/messages.

bash

sudo grep dhcpd /var/log/syslog

En un servidor DHCP de Windows:

    Abre el Visor de Eventos.
    Navega a Registros de Windows > Sistema.
    Busca eventos relacionados con DHCP.

6. Comprobar el Archivo de Leases del DHCP en el Host

Si estás utilizando un servidor DHCP en el mismo host (por ejemplo, dnsmasq o isc-dhcp-server), puedes revisar el archivo de leases para ver qué dispositivo ha recibido qué IP.

Para isc-dhcp-server:

bash

cat /var/lib/dhcp/dhcpd.leases

Para dnsmasq:

bash

cat /var/lib/misc/dnsmasq.leases

7. Usar Comandos Específicos de NetworkManager

Si estás usando NetworkManager, puedes obtener detalles sobre el servidor DHCP directamente desde nmcli.

Comando:

bash

nmcli device show enp3s0 | grep DHCP4

Ejemplo de Salida:

makefile

IP4.DHCP.SERVER:                             172.30.1.1

Esto indica que el servidor DHCP que asignó la IP es 172.30.1.1.
8. Identificar el Servidor DHCP mediante Scanning de Red

Puedes usar herramientas como nmap para identificar qué dispositivos en la red están ofreciendo servicios DHCP.

Instalación de nmap:

    En Linux:
    
    bash
    
    sudo apt install nmap

Uso de nmap para detectar servidores DHCP:

bash

sudo nmap --script broadcast-dhcp-discover

Este comando envía una solicitud DHCP y espera respuestas, identificando qué dispositivos están respondiendo como servidores DHCP.
Resumen de Pasos Recomendados

    Verifica la Configuración de Red del Dispositivo:
        Usa nmcli, ip, ipconfig /all, o herramientas similares para identificar el servidor DHCP desde el dispositivo que ha recibido la IP.
    
    Revisa la Interfaz de Administración del Router:
        Accede al router para ver la lista de dispositivos conectados y los servidores DHCP activos.
    
    Utiliza Herramientas de Análisis de Red:
        Emplea Wireshark para capturar y analizar tráfico DHCP, o usa dhcping para verificar servidores específicos.
    
    Inspecciona la Tabla ARP y Logs del Servidor DHCP:
        Usa comandos como arp -a y revisa los logs del servidor DHCP para obtener información detallada.
    
    Utiliza nmap para Escanear la Red:
        Identifica qué dispositivos están ofreciendo servicios DHCP en tu red local.

Consejos Adicionales

    Asegúrate de Tener los Permisos Adecuados:
        Muchas de estas operaciones requieren privilegios de administrador o root.
    
    Mantén Documentada tu Red:
        Si tienes múltiples servidores DHCP, asegúrate de documentar su propósito y configuración para evitar conflictos.
    
    Seguridad de DHCP:
        Considera implementar medidas de seguridad como DHCP Snooping en redes más grandes para prevenir ataques de servidores DHCP no autorizados.

Conclusión

Identificar el origen de la asignación de direcciones IP en una red local implica revisar la configuración de red de los dispositivos, inspeccionar el router o servidores DHCP, y utilizar herramientas de análisis de red para capturar y analizar tráfico DHCP. Siguiendo los métodos descritos anteriormente, podrás determinar de manera efectiva qué dispositivo o servicio está asignando direcciones IP en tu red.