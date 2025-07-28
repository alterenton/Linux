# Instalación de Arch con UEFI
Pasos para instalar
## Correcciones para comodidad
```
localectl list-keymaps
```
```
loadkeys es
```
```
ls /usr/share/kbd/consolefonts/
```
```
setfont ter-132b
```
## Conexión a Internet
```
ping archlinux.org
```
Si funciona pasamos al siguiente paso.
Si no funciona usamos:
```
ip a
```
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
