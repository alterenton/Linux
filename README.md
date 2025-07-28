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
## Crear subvolumes
```
mount /dev/sda2 /mnt
btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
umount /mnt
```
## Montar subvolumenes
```
mount -o noatime,compress=zstd,subvol=@ /dev/sda2 /mnt
mkdir -p /mnt/{boot,home}
mount -o noatime,compress=zstd,subvol=@home /dev/sda2 /mnt/home
mount /dev/sda1 /mnt/boot
```
## Instalar base
```
pacstrap -K /mnt base linux linux-firmware btrfs-progs
```
## Generar fstab
```
genfstab -U /mnt >> /mnt/etc/fstab
```
## Entras al sistema
```
arch-chroot /mnt
```
## Localización y horarios
```
ln -sf /usr/share/zoneinfo/America/Lima /etc/localtime
hwclock --systohc
```
Buscando la opción en_US.UTF-8 para descomentar
```
pacman -S micro
micro /etc/locale.gen
```
Resguardamos con
```
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```
## Host y red
```
echo "bloodbound" > /etc/hostname
pacman -S networkmanager
systemctl enable NetworkManager
```
## Contraseña
```
passwd
```
## Bootloader
```
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ArchGrub
grub-mkconfig -o /boot/grub/grub.cfg
```
## Usuario
```
useradd -m -G wheel enton
passwd enton
```
Vamos a darle privilegios descomentando el **wheel**
```
pacman -S sudo
micro /etc/sudoers
```
## Reiniciamos
```
exit
umount -R /mnt
reboot
```
