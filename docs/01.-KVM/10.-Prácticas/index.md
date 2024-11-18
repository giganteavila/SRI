# Ejercicios KVM

## Ejercicio 1: Verificación del Soporte de Virtualización en el Host
**Tarea**: Ejecuta el comando `egrep -E -c '(vmx|svm)' /proc/cpuinfo` en tu máquina host para verificar si el procesador soporta virtualización por hardware (Intel VT o AMD-V).

**Instrucciones**:

- Abre una terminal en tu sistema Linux.
- Ejecuta el comando indicado.
- Si el resultado es mayor que 0, la virtualización está habilitada.
 **Objetivo**: Verificar que el hardware soporta la creación de máquinas virtuales con KVM.

---
## Ejercicio 2: Instalación de KVM y Herramientas de Virtualización
**Tarea**: Instala el paquete qemu-kvm, libvirt-bin, y virt-manager en una distribución Linux (por ejemplo, Ubuntu) y habilita el servicio de libvirtd.

**Instrucciones**:

- Ejecuta `apt install qemu-system libvirt-clients libvirt-daemon-system`.

- Habilita el servicio con `sudo systemctl enable libvirtd` y luego inícialo con sudo `systemctl start libvirtd`.

- Verifica la correcta instalación ejecutando `virsh list --all`.

**Objetivo**: Preparar el sistema para crear y administrar máquinas virtuales.

---
## Ejercicio 3: Creación de una Máquina Virtual desde la CLI

**Tarea**: Utiliza el comando `virt-install` para crear una máquina virtual Ubuntu desde la línea de comandos con un disco de 10GB y 2GB de RAM.

**Instrucciones**:

- Descarga una imagen de Ubuntu Server (ISO).
- Ejecuta el siguiente comando:

```bash
sudo virt-install --name UbuntuVM \
  --ram 2048 \
  --vcpus 2 \
  --disk path=/var/lib/libvirt/images/ubuntu.qcow2,size=10 \
  --location ~/<nombre_distribución>,kernel=casper/vmlinuz,initrd=casper/initrd \
  --os-variant ubuntu24.04 \
  --network bridge=virbr0 \
  --graphics none \
  --console pty,target_type=serial \
  --extra-args 'console=ttyS0,115200n8 serial'
```

Sigue el proceso de instalación a través de la consola.

**Objetivo**: Crear y administrar una máquina virtual usando solo la CLI.

---

## Ejercicio 4: Creación de un Disco Virtual Adicional para una MV Existente

**Tarea**: Añadir un segundo disco virtual de 5GB a una máquina virtual existente.
**Instrucciones**:

- Detén la máquina virtual con `virsh shutdown <nombreMV>`.

- Usa el comando `virsh edit <nombreMV>` para editar la configuración XML de la máquina.

- Añade la siguiente sección en el archivo XML:

```xml
<disk type='file' device='disk'>
   <driver name='qemu' type='qcow2'/>
   <source file='/var/lib/libvirt/images/disk2.qcow2'/>
   <target dev='vdb' bus='virtio'/>
</disk>
```
- Guarda el archivo y arranca la MV de nuevo.

**Objetivo**: Familiarizarse con la administración de almacenamiento adicional en una MV.
**¡ OJO !:**: Debes crear el disco virtual previamente. 

---

## Ejercicio 5: Migración en Vivo de una MV a Otro Host

**Tarea**: Realizar una migración en vivo de una máquina virtual desde un host KVM a otro sin detenerla.
**Instrucciones**:
- Asegúrate de que ambos hosts están configurados correctamente para permitir la migración.
- Ejecuta el siguiente comando desde el host origen:

```bash
$ virsh migrate --live <nombreMV> qemu+ssh://user@destination/system
```
- Verifica que la MV sigue ejecutándose en el host destino con virsh list.

**Objetivo**: Ejecutar una migración en vivo para mejorar la disponibilidad y el mantenimiento de sistemas.

**¡ OJO !**: Antes de realizar la migración en vivo, asegúrate de que los siguientes aspectos están configurados correctamente en ambos hosts (origen y destino):

- Ambos hosts deben usar KVM/QEMU con libvirt.
- **SSH sin contraseña**: Configura la autenticación sin contraseña entre el host origen y destino mediante claves SSH.
- **Almacenamiento compartido o accesible**: Asegúrate de que el almacenamiento de la máquina virtual (imágenes de disco) está accesible desde ambos hosts (por ejemplo, a través de NFS, iSCSI, etc.). La migración en vivo requiere que los archivos de disco de la VM sean accesibles desde el host de destino.
- Ambos hosts deben tener las **mismas versiones** de QEMU/KVM y libvirt para asegurar la compatibilidad.

---

## Ejercicio 6: Uso de Snapshots en MVs

**Tarea**: Crear un snapshot de una máquina virtual en funcionamiento y restaurarla a ese punto después de realizar cambios en el sistema operativo.
**Instrucciones**:

- Para crear un snapshot:

```bash
$virsh snapshot-create-as --domain <nombreMV> --name "snapshot1" --description "Estado limpio antes de actualizaciones"
```
Realiza algunos cambios en la MV, como instalar software o modificar configuraciones.
Restaura el estado anterior usando:

```bash
virsh snapshot-revert <nombreMV> snapshot1
```
**Objetivo**: Aprender a utilizar snapshots para la recuperación rápida de MVs.

---

## Ejercicio 7: Configuración de NAT en la Red Virtual

**Tarea**: Configurar NAT para las máquinas virtuales, permitiendo que accedan a Internet a través del host.

**Instrucciones**:

- Edita el archivo de red default con el siguiente comando:
```bash
virsh net-edit default
```
- Configura el modo de red en NAT, verificando la conectividad de la MV hacia el exterior.

Usa `ping` desde la MV para verificar que puede acceder a Internet.

**Objetivo**: Configurar redes virtuales para conectar MVs con redes externas.

---

## Ejercicio 8: Creación de una Máquina Virtual con Virt-Manager

**Tarea**: Utiliza la interfaz gráfica virt-manager para crear una máquina virtual Windows 10 con 4GB de RAM y un disco de 20GB.
**Instrucciones**:

- Abre virt-manager y selecciona la opción para crear una nueva máquina.
- Sigue las instrucciones para elegir la imagen de instalación y configurar los recursos de la máquina.
- Inicia la instalación de Windows desde la interfaz gráfica.

**Objetivo**: Familiarizarse con la administración de MVs desde la interfaz gráfica.

---

## Ejercicio 9: Clonación de una Máquina Virtual

**Tarea**: Clona una máquina virtual existente y cámbiale el nombre y la dirección MAC de la interfaz de red.
**Instrucciones**:

- Ejecuta el comando:
```bash
$ virt-clone --original <nombreMV> --name <nombreClon> --file /var/lib/libvirt/images/<nombreClon>.qcow2
```
Edita la dirección MAC para evitar conflictos de red:

```bash
$ virsh edit <nombreClon>
```
- Busca la sección de la interfaz de red y cambia el valor de la dirección MAC.

**Objetivo**: Dominar la clonación y personalización de máquinas virtuales.

---

## Ejercicio 10: Implementación de Contenedores LXC
**Tarea**: Crear un contenedor LXC en el entorno KVM, configurando las capacidades de red compartida con el host.
**Instrucciones**:
- Instala LXC con sudo apt install lxc.
- Crea un contenedor básico:
```bash
lxc-create -n mycontainer -t ubuntu
```
- Configura el acceso a la red compartida entre el contenedor y el host.
- Inicia el contenedor con lxc-start.

**Objetivo**: Integrar la tecnología de contenedores dentro del entorno de virtualización KVM.

