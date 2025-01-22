# Instalación de zsh en ubuntu

Para instalar Zsh en Ubuntu, sigue los siguientes pasos:

- Abre una terminal en tu sistema Ubuntu.
- Actualiza la lista de paquetes de Ubuntu con el siguiente comando:
```
$ sudo apt update
```
    
- Ahora, instala Zsh con el siguiente comando:
```
 sudo apt install zsh
``` 
- Después de la instalación, verifica que la versión de Zsh instalada es la última disponible con:
``` 
zsh --version
```
- Para configurar Zsh como tu shell predeterminada, utiliza el siguiente comando:
```
sudo chsh -s $(which zsh)
```
- Cierra la sesión actual (saliendo del modo gráfico) e inicia sesión nuevamente para que los cambios surtan efecto.
- Instalar Oh-my-zsh según las instrucciones de su [página web](https://ohmyz.sh/) es decir, ejecutar el comando:
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
- Abre tu terminal y ejecuta el siguiente comando para abrir el archivo de configuración de Oh My Zsh:

```
vi ~/.zshrc
```

- Busca la línea que comienza con `ZSH_THEME` y cambia el valor después del signo igual por el nombre del te
  ma que quieres utilizar. Por ejemplo, si quieres utilizar el tema agnoster, la línea debería verse así:
```
ZSH_THEME="agnoster"
```
- Guarda los cambios y cierra el archivo de configuración.

- Sal de tu sesión de terminal actual y vuelve a iniciarla para que los cambios tengan efecto.

[instalacion_del_plugin_en_ZSH](instalacion_del_plugin_en_ZSH.md)
