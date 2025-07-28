# Instalación de Arch con UEFI
Pasos para instalar
## Correcciones para comodidad
Para encajar los símbolos de distribuciones de teclado diferentes
```
localectl list-keymaps
loadkeys es
```
Para mejorar el tamaño de letra para visualizar
```
ls /usr/share/kbd/consolefonts/
setfont ter-132b
```
## Conexión a Internet
Comprobamos si tenemos internet
```
ping archlinux.org
```
**Si funciona, pasamos al siguiente paso.**
*Si no funciona usamos:*
```
ip a
```
Con esto recordamos el adaptador wifi. Luego,
```
iwctl
```
Además, colocamos nuestra red wifi junto a su clave
```
station [nombredeladaptador] connect [nombredelared] password [clavepassword]
exit
```
## Partición del disco
Observamos los discos
```
lsblk
```
Recordemos el nombre del disco a trabajar
```
cfdisk /dev/[nombredeldisco]
```
Aceptamos poner **GPT**, si previamente tenía particiones podemos eliminarlos hasta dejarlo en su estado más vacío. Vamos a seleccionar el disco y colocamos un tamaño de **512M**, luego en *tipo* buscamos poner **EFI System**. El resto del disco será para el sistema, en tipo colocamos **Linux x86-64**, al finalizar todo damos en **write** para escribir con un **yes**, terminamos con **exit** aceptando.
## Formatear particiones
Usualmente los nombres de los discos es **sda**, pero esto puede cambiar. Para formatear el boot
```
mkfs.fat -F 32 /dev/sda1
```
Ahora, como hay un sistema de archivos novedoso, vamos a actualizarlo con
```
mkfs.btrfs /dev/sda2
```
