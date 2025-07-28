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
Si funciona, pasamos al siguiente paso.
Si no funciona usamos:
```
ip a
```
Con esto recordamos el adaptador wifi.
```
iwctl
```
```
station [nombredeladaptador] connect [nombredelared] password [clavepassword]
```
```
exit
```
## Partición del disco
```
lsblk
```
```
cfdisk /dev/[nombredeldev]
```
Ingresamos GPT por ser UEFI, 
## Formatear particiones
1
