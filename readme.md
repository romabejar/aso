# Repositori per l'examen d'ASO de laboratori #

## Comandes practica 1 ##
		- lspci
- lsusb
- dmesg

## Instalació del sistema operatiu ##

1) Particionar disc
   sudo fdisk /dev/sdb 
	o (new MBR partition table)
	n (new partition)
		p (primary)
		1 (partition number)
		default
		+100G (100 GiB)
	n
		p
		2
		default
		+16G
	n
		e
		default
		end of disk (default)
	crear particions logiques

	w (write and exit)

2) formatejar particions
	sudo mkfs.ext4 /dev/sdb1
        sudo mkswap /dev/sdb2
	sudo swapon /dev/sdb2
3) muntar el disk en el sistema
	mkdir /linux
	mount /dev/sdb1 /linux
	mkdir /linux/dev
	mount -o bind /dev /linux/dev
	
		
4) descargar el sistema i descomprimirlo en el disk
 	wget -O- ftp://asoserver.pc.ac.upc.edu/packages/aso-install.tar.gz | tar zxf -

5) configurar el fstab
	vim /etc/fstab (from the disk where the installation is being performed)
	
Add swap partition:
	/dev/sdb2	none	swap	defaults	0	0
Add root partition
	/dev/sdb1	/	ext4	defaults	0	1
Add other partitions:
	/dev/sdb5	/usr/local	ext4	defaults	0	2
	/dev/sdb6	/home	ext4	defaults	0	2	

6) change root
	chroot /linux
	
7) configure keyboard
	dpkg-reconfigure console-data
	dpkg-reconfigure keyboard-configuration
	
8) Install grub
	grub-install /dev/sdb
	update-grub

9) umount everything (remember /linux/dev )
10)restart pc and start from device.

11)  connect to internet
ifdown eth0
vim /etc/network/interfaces

auto eth0
allow-hotplug eth0
	iface eth0 inet dhcp
	
