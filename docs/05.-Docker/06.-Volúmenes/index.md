# 06.- Volúmenes 

**Los contenedores son efímeros**, es decir, los ficheros, datos y configuraciones que creamos en los contenedores sobreviven a las paradas de los mismos pero, sin embargo, son destruidos si el contenedor es destruido.

## Los datos en los contenedores 

![docker](../img/types-of-mounts.png)

Ante la situación anteriormente descrita Docker nos proporciona varias soluciones para persistir los datos de los contenedores. En este curso nos vamos a centrar en las dos que considero que son más importantes:

- Los **volúmenes docker**.
- Los **bind mount**
- Los **tmpfs mounts**: Almacenan en memoria la información. (No lo vamos a ver en este curso)