Los ficheros de configuración de las redes los encontraremos en:

`/etc/libvirt/qemu/networks/*.xml`

El componete principal del las redes libvirt son los  “Switches-Virtuales”, conmutador de red virtual, al que las máquinas conectaran sus NIC's virtuales (vNIC).

![img](https://www.pinguytaz.net/wp-content/uploads/2021/08/image.png)

Si vemos nuestros interfaces locales veremos que tenemos uno o varios dispositivos `virbrX`, esto son los Switches-Virtuales, y un dispositivo (vnetX) por cada vNIC que este levantado en las maquinas virtuales. Aunque esto nos lo proporciona directamente *virt-manager*, más adelante veremos exáctamente cómo crear las redes, en la opción **Editar→Detalles_Conexión** dentro de la carpeta “Redes Virtuales”.

![img](https://www.pinguytaz.net/wp-content/uploads/2021/08/image-1.png)

Desde la línea de comandos podremos crear estos dispositivos de la siguiente forma:

```
brctl addbr // Switch Virtual

// Los vNICs tipo TIN/TAP
ip tuntap add dev mode tap
brctl addif
```