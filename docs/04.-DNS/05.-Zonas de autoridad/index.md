# 05.-Zonas de Autoridad

Un **servidor DNS** almacena información acerca de ciertas partes del **espacio de nombres de dominio**, conocidas como **zonas**.

- Una **zona** no necesariamente coincide con un dominio completo.
- Se dice que un servidor DNS tiene **autoridad sobre una zona** cuando administra dicha parte del espacio de nombres.
- Un servidor de nombres puede tener autoridad sobre varias zonas.

## Mapa de Dominio o de Zona

La información relacionada con la resolución de nombres de un dominio específico se guarda en un fichero llamado **mapa del dominio o de zona**.

En el **mapa de una zona o dominio**, se encuentran, entre otros datos:

1. Los **nombres de las máquinas del dominio**, junto con sus respectivas **direcciones IP**.
2. Los **nombres de los subdominios directos**, junto con las **direcciones IP** de los servidores DNS que gestionan esos subdominios.

## Administración del Mapa de Dominio

- El **administrador de sistemas** del dominio edita y mantiene el mapa de dominio.
- Este fichero se almacena en la máquina que opera como **servidor DNS** del dominio.

## Servidor DNS con Múltiples Ficheros de Zona

Un **servidor DNS** que gestione varios ficheros de zona será capaz de administrar todos los dominios correspondientes a dichos ficheros.
