---
layout: post
url: https://medium.com/@/649a7197baad
title: Xvisor — première mise en œuvre
subtitle: Xvisor est un hyperviseur open source en licence GPLv2. Cet hyperviseur
  est de type-1 ou natif, c’est-à-dire qu’il s’exécute directement…
slug: xvisor-première-mise-en-œuvre
description: 
tags:
- hypervisors
- xvisor
- emulator
- raspberry-pi
- embedded-systems
author: dagar
image: assets/images/posts/0*TCVzCRqxXLRrBwoY.jpg
---
🇫🇷 This article is available in French only, part of our cross-post series from our friends at Linux embedded.

Xvisor est un hyperviseur open source en licence GPLv2. Cet hyperviseur est de type-1 ou natif, c’est-à-dire qu’il s’exécute directement sur la cible sans couche d’abstraction intermédiaire. Il existe un autre type d’hyperviseur, de type-2, qui s’exécute au-dessus d’un système d’exploitation.

Xvisor est développé depuis 2014, et est porté principalement sur des cibles ARM (v5, v6, v7a v7a-ve v8a), x86 et x86_64.

Nous allons mettre en œuvre Xvisor sur deux cibles : une Raspberry Pi 3 et une machine virtuelle Qemu pour deux architectures différentes. Pour cela, nous partons d’un environnement de développement vierge sous Debian. Cela ne devrait poser aucun problème sous un autre environnement tel que Ubuntu ou autres. On suppose que Git est installé sur la machine.

# Aperçu de l’architecture de Xvisor

Avant de décrire l’architecture de Xvisor, nous allons récupérer les sources en clonant le dépôt sur la machine locale. Xvisor est hébergé sous Github [https://github.com/xvisor/xvisor.](https://github.com/xvisor/xvisor.)

# Arborescence

La description de l’architecture générale de Xvisor est disponible dans le répertoire docs/DesignDoc

    $ tree -d -L 1
    .
    ├── arch
    ├── commands
    ├── core
    ├── daemons
    ├── docs
    ├── drivers
    ├── emulators
    ├── libs
    ├── tests
    └── tools

On retrouve une architecture ressemblant à celle d’une distribution Linux.

* arch : Les architectures cibles sur lesquelles Xvisor fonctionne;
* commands : Ce dossier contient les commandes qui peuvent être disponibles sous l’invite de commande Xvisor;
* core : La mécanique Xvisor;
* daemons : La gestion terminaux, telnet;
* docs : documentation;
* drivers : Les drivers disponibles classés par type de driver, chaque sous-dossier implémente un driver générique et des drivers dédiés;
* emulators : Ce dossier contient les émulateurs classés par type. On y trouve des émulateurs génériques ou spécifiques à un périphérique;
* libs : On y trouve des utilitaires pour le test, le debug, du chiffrement, la pile lwIP, etc.
* tests : Ce répertoire contient des exemples de configurations complètes par <architecture>/<carte>/<invité>. La carte peut être une carte existante du commerce ou alors simplement un jeu d’instruction. Par exemple : Raspberry Pi 3 avec invité natif — tests/arm64/virt-v8/basic — , Raspberry Pi 3 avec Linux — tests/arm64/virt-v8/linux — , Raspberry Pi 2 avec invité natif — tests/arm32/virt-v7/basic — , SABRE Lite (Nitrogen6X) avec Linux — tests/arm32/sabrelite/linux —
* tools : On y trouve divers outils pour générer Xvisor; un script pour patcher les jeux d’instruction sans extension de virtualisation, le compilateur pour les « device tree », etc.

Xvisor construit des machines virtuelles sur lesquelles tournent des invités. Chaque machine virtuelle est constituée d’un espace mémoire propre et de CPUs virtuels (les VCPUs). Les VCPUs sont divisés en 2 groupes : ceux dédiés aux invités les VCPUs normaux et ceux dédiés au fonctionnement du contrôleur (Xvisor), les VCPUs orphelins (o_VCPU).

# Emulateurs

Xvisor est déployé sur plusieurs cartes mais peu de périphériques sont implémentés. On trouve cependant systématiquement une console série : son émulateur et driver.

Les invités peuvent manipuler 3 types de périphériques :

1. Les périphériques émulés : ces périphériques sont entièrement émulés avec du logiciel et imitent de vrais périphériques;
2. Les périphériques para-virtualisés : ces périphériques sont émulés avec du logiciel mais sont imaginaires. Il est toutefois indispensable de minimiser les couches logicielles pour des soucis d’efficacité;
3. Les périphériques « pass-through »: ce sont de vrais périphériques, mappés sur un invité. L’intérêt de ce type de périphérique est la performance.

Les émulateurs peuvent être instanciés pour plusieurs invités, chacun relié à un driver différent ou identique, dans la limite des ressources matérielles disponibles.

Le principe d’un émulateur est de simuler un périphérique et donc de proposer une implémentation de ses registres mappés dans la mémoire propre d’une machine virtuelle. Lors d’un accès à ces registres virtuels l’hyperviseur intercepte l’instruction (écriture / lecture) et la route vers un driver.

# Mise en œuvre

La mise en œuvre de Xvisor est assez simple car tout est décrit dans les fichiers docs/<architecture>/<macible>.txt.

Nous choisissons deux exemples :

* Qemu : voir le fichier docs/arm/foundation-v8.txt;
* Raspberry Pi 3 : voir le fichier docs/arm/bcm2837-raspi3.txt.

Les documentations sont assez complètes, mais on voit souvent des primo-accédants posant des questions pratiques sur le forum. Nous allons donc détailler la démarche pour un premier démarrage de Xvisor.

Nous commençons par la description commune aux deux exemples, puis nous réaliserons les 2 exemples.

Ces exemples mettent en œuvre Xvisor sur lequel nous aurons un invité natif qui nous permettra de lancer Linux.

# Préparation de l’environnement

Nous allons créer une arborescence contenant tous les utilitaires indispensables à Xvisor.

    .
    └── xvisor-tree
    ├── linux
    ├── u-boot
    └── xvisor

Pour la suite on suppose que cette arborescence se trouve dans : $HOME/workspace

    $ cd $HOME/workspace
    $ mkdir -p xvisor-tree/tools
    $ mkdir -p xvisor-tree/linux/linux-build
    $ mkdir -p xvisor-tree/u-boot/u-boot-build
    $ mkdir -p xvisor-tree/busybox

Nous clonons Xvisor immédiatement dans notre environnement:

    $ cd $HOME/workspace/xvisor-tree
    $ git clone https://github.com/xvisor/xvisor.git xvisor

Nous aurons besoin des outils suivants :

* flex
* bison
* genext2fs
* telnet

Ces outils sont tous disponibles sous la forme de paquets sur toute les distributions Linux. La commande ci-dessous permet de les installer sous Debian :

    $ sudo apt-get install flex bison genext2fs telnet

# Chaîne de compilation

Nous choisissons une chaîne de compilation pour le jeu d’instruction ARMv8 et à destination d’une application de type linux, c’est-à-dire avec les bibliothèques adaptées (glibc, etc.). L’ensemble des compilateurs pour architecture ARM est décrit à la page suivante :

[https://collaborate.linaro.org/display/TCWGPUB/ARM+and+AArch64+Target+Triples](https://collaborate.linaro.org/display/TCWGPUB/ARM+and+AArch64+Target+Triples)

    $ cd $HOME/workspace/xvisor-tree/tools
    $ wget https://releases.linaro.org/components/toolchain/binaries/latest-6/aarch64-linux-gnu/gcc-linaro-6.3.1-2017.02-x86_64_aarch64-linux-gnu.tar.xz
    $ tar xvf gcc-linaro-6.3.1-2017.02-x86_64_aarch64-linux-gnu.tar.xz
    $ ln -s gcc-linaro-6.3.1-2017.02-x86_64_aarch64-linux-gnu aarch64-linux-gnu
    $ GCC_AARCH64_LINUX_GNU=$HOME/workspace/xvisor-tree/tools/aarch64-linux-gnu
    $ PATH=$PATH:$GCC_AARCH64_LINUX_GNU/bin

On vérifie que le compilateur est bien accessible:

    $ cd $HOME/workspace/xvisor-tree $ aarch64-linux-gnu-gcc --version aarch64-linux-gnu-gcc (Linaro GCC 6.3-2017.02) 6.3.1 20170109 Copyright (C) 2016 Free Software Foundation, Inc. This is free software; see the source for copying conditions. There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE

On exporte une variable d’environnement pour la compilation:

    $ export CROSS_COMPILE=aarch64-linux-gnu-

# Noyau Linux

Nous lancerons un noyau Linux au-dessus de Xvisor, nous choisissons la version 4.9, celle testée dans Xvisor. Ce noyau est minimal, nous avons besoin de quelques utilitaires pour jouer avec.

    $ cd $HOME/workspace/xvisor-tree/linux
    $ wget https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.9.11.tar.xz $ tar xvf linux-4.9.11.tar.xz

### Busybox

Busybox est un environnement permettant de générer un ensemble d’utilitaires (ls, ps, mkdir, rm, dpkg, adduser, etc.) pouvant être embarqués dans un milieu pauvre en ressources. Ces utilitaires sont ceux que tout utilisateur Linux classique utilise au quotidien.

    $ cd $HOME/workspace/xvisor-tree/busybox
    $ wget https://www.busybox.net/downloads/busybox-1.25.1.tar.bz2 $ tar xvf busybox-1.25.1.tar.bz2

On prépare l’environnement Busybox avec le fichier de configuration fourni par Xvisor.

    $ cd $HOME/workspace/xvisor-tree/busybox/busybox-1.25.1
    $ cp $HOME/workspace/xvisor-tree/xvisor/tests/common/busybox/busybox-1.25.1_defconfig ./.config $ make oldconfig
    $ make install
    $ mkdir -p ./_install/etc/init.d
    $ mkdir -p ./_install/dev
    $ mkdir -p ./_install/proc
    $ mkdir -p ./_install/sys
    $ ln -sf /sbin/init ./_install/init
    $ cp -f $HOME/workspace/xvisor-tree/xvisor/tests/common/busybox/fstab ./_install/etc/fstab
    $ cp -f $HOME/workspace/xvisor-tree/xvisor/tests/common/busybox/rcS ./_install/etc/init.d/rcS
    $ cp -f $HOME/workspace/xvisor-tree/xvisor/tests/common/busybox/motd ./_install/etc/motd
    $ cp -f $HOME/workspace/xvisor-tree/xvisor/tests/common/busybox/logo_linux_clut224.ppm ./_install/etc/logo_linux_clut224.ppm
    $ cp -f $HOME/workspace/xvisor-tree/xvisor/tests/common/busybox/logo_linux_vga16.ppm ./_install/etc/logo_linux_vga16.ppm
    $ cd ./_install; find ./ | cpio -o -H newc > ../rootfs.img; cd -

# Compilation de Xvisor

Xvisor est déjà cloné, voir au-dessus.

Selon la cible choisie, toutes les instructions pour construire Xvisor sont décrites dans :

    docs/arm

Avant toute chose, il faut ajouter le sous-module git tools/dtc avec la commande suivante :

    $ cd $HOME/workspace/xvisor-tree/xvisor
    $ git submodule init
    $ git submodule update

La suite est décomposée en 3 parties :

1. Partie commune : cette partie est obligatoire pour faire tourner l’un ou l’autre des exemples suivants;
2. Qemu : cet exemple permet de lancer Xvisor dans un environnement émulé;
3. Raspberry Pi 3 : cet exemple nécessite une carte Raspberry Pi 3.

# Partie commune

Xvisor est constitué des images suivantes :

* L’image de Xvisor : **vmm.bin**;
* le fichier **DTB** qui décrit les paramètres bas-niveau des invités, dans notre cas : **one_guest_virt-v8.dtb**;
* L’image de Xvisor contenant les invités : **disk.img**.

### Création de vmm.bin

Pour construire vmm.bin nous utilisons le label « generic-v8-defconfig » qui correspond à l’architecture choisie (commune à nos deux exemple).

    $ cd $HOME/workspace/xvisor-tree/xvisor
    $ make ARCH=arm generic-v8-defconfig
    $ make; make dtbs

A ce stade nous avons construit vmm.bin.

### Création de disk.img et du fichier DTB

Cette étape est la plus fastidieuse puisque nous avons un grand nombre de commandes à taper.

    $ cd $HOME/workspace/xvisor-tree/xvisor$
     make -C tests/arm64/virt-v8/basic
    $ cp tests/arm64/virt-v8/linux/linux-4.9_defconfig ../linux/linux-build/.config $ cd $HOME/workspace/xvisor-tree/linux/linux-4.9.11
    $ make O=../linux-build ARCH=arm64 oldconfig
    $ make O=../linux-build ARCH=arm64 Image dtbs
    $ cd $HOME/workspace/xvisor-tree/xvisor
    $ mkdir -p ./build/disk/tmp
    $ mkdir -p ./build/disk/system
    $ cp -f ./docs/banner/roman.txt ./build/disk/system/banner.txt
    $ cp -f ./docs/logo/xvisor_logo_name.ppm ./build/disk/system/logo.ppm
    $ mkdir -p ./build/disk/images/arm64/virt-v8
    $ ./build/tools/dtc/bin/dtc -I dts -O dtb -o ./build/disk/images/arm64/virt-v8x2.dtb ./tests/arm64/virt-v8/virt-v8x2.dts
    $ cp -f ./build/tests/arm64/virt-v8/basic/firmware.bin ./build/disk/images/arm64/virt-v8/firmware.bin
    $ cp -f ./tests/arm64/virt-v8/linux/nor_flash.list ./build/disk/images/arm64/virt-v8/nor_flash.list
    $ cp -f ./tests/arm64/virt-v8/linux/cmdlist ./build/disk/images/arm64/virt-v8/cmdlist
    $ cp -f $HOME/workspace/xvisor-tree/linux/linux-build/arch/arm64/boot/Image ./build/disk/images/arm64/virt-v8/Image $ ./build/tools/dtc/bin/dtc -I dts -O dtb -o ./build/disk/images/arm64/virt-v8/virt-v8.dtb ./tests/arm64/virt-v8/linux/virt-v8.dts
    $ cp -f $HOME/workspace/xvisor-tree/busybox/busybox-1.25.1/rootfs.img ./build/disk/images/arm64/rootfs.img
    $ genext2fs -B 1024 -b 16384 -d ./build/disk ./build/disk.img

Enfin, nous avons les 2 images et le fichier **dtb** (one_guest_virt-v8.dtb) qui nous permettent de lancer Xvisor au moins dans un émulateur, c’est ce que nous faisons maintenant. Le fichier dtb est la description du matériel sous la forme d’un « Device Tree Blob », la documentation sur ce format est largement disponible sur le web. Ce format permet de décrire l’architecture matérielle sur laquelle va être lancée Xvisor et par la suite Linux.

Nous utilisons dans la suite :

* vmm.bin : ./build/vmm.bin
* disk.img : ./build/disk.img
* one_guest_virt-v8.dtb : ./build/arch/arm/board/generic/dts/foundation-v8/gicv2/one_guest_virt-v8.dtb

Pour ceux qui préfèrent utiliser directement une Raspberry Pi 3 il suffit de sauter l’étape suivante et de passer directement au paragraphe Raspberry Pi 3.

# Qemu

Nous proposons de décrire un exemple basé sur les machines fournies par ARM : [Fixed Virtual Platforms](https://developer.arm.com/products/system-design/fixed-virtual-platforms).

Cet exemple est décrit dans : docs/arm/foundation-v8.txt

Dans la suite nous créons une version de Xvisor pour la plateforme « ESL: Fast Models: ARMv8-A Foundation Platform » disponible à l’adresse [https://silver.arm.com/browse/FM00A](https://silver.arm.com/browse/FM00A) (FM000-KT-00035-r10p3–26rel0.tgz).

La machine virtuelle est installée dans workspace/tools.

    $ cd $HOME/workspace/xvisor-tree/tools
    $ tar xvf FM000-KT-00035-r10p3-26rel0.tgz

On construit le fichier **foundation_v8_boot.axf** permettant de démarrer la machine virtuelle, c’est un fichier au format ELF avec des informations de debug.

    $ cd $HOME/workspace/xvisor-tree/xvisor
    $ ${CROSS_COMPILE}gcc -nostdlib -nostdinc -e _start -Wl,--build-id=none -Wl,-Ttext=0x80000000 -DGENTIMER_FREQ=100000000 -DUART_PL011 -DUART_PL011_BASE=0x1c090000 -DGICv2 -DGIC_DIST_BASE=0x2c001000 -DGIC_CPU_BASE=0x2c002000 -DSPIN_LOOP_ADDR=0x8000fff8 -DIMAGE=./build/vmm.bin -DDTB=./build/arch/arm/board/generic/dts/foundation-v8/gicv2/one_guest_virt-v8.dtb -DINITRD=./build/disk.img ./docs/arm/foundation_v8_boot.S -o ./build/foundation_v8_boot.axf

Nous lançons la machine virtuelle.

    $ $HOME/workspace/xvisor-tree/tools/Foundation_Platformpkg/models/Linux64_GCC-4.7/Foundation_Platform --arm-v8.1 --image ./build/foundation_v8_boot.axf --network=nat

La machine démarre avec deux fenêtres : la première, la fenêtre CLCD dans laquelle se trouve des informations sur l’état de la machine et la seconde est un terminal dans laquelle nous voyons Xvisor démarrer.

Pour utiliser Xvisor, rendez-vous au paragraphe « Utilisation de Xvisor ».

# Raspberry Pi 3

Dans cette partie nous réutiliserons les binaires compilés au-dessus en les adaptant au milieu de la carte Raspberry Pi. Nous construisons également une image supplémentaire contenant un bootloader (dans le cas précédent, Qemu s’est occupé du chargement de Xvisor), en l’occurence U-Boot.

Nous construisons les images suivantes :

* L’image de U-Boot : **u-boot.bin**;
* L’image de Xvisor : **uvmm.bin**;
* le fichier **DTB** qui décrit les paramètres bas-niveau des invités, dans notre cas : **one_guest_virt-v8.dtb**;
* L’image de Xvisor contenant les invités : **udisk.img**.

Mais avant tout, il faut préparer une carte SD prête à recevoir l’image Xvisor.

Si vous avez déjà installé une Raspbian ou toute autre distribution permettant de démarrer votre Rapsberry Pi 3, alors ce qui suit n’est pas utile.

Dans l’autre cas voici une démarche simple.

On récupère une image de Raspbian, puis on la transfère en mode bloc sur la carte SD. Tout est clairement expliqué sur le site de Raspberry:

[https://www.raspberrypi.org/documentation/installation/installing-images/linux.md](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md)

La carte est constituée de 2 partitions, une partition de boot au format FAT, et une partition data au format ext4.

Ces deux partitions sont référencées de la façon suivante :

* boot : /media/<user>/boot
* data : /media/<user>/data

La carte est prête à accueillir les 4 images que nous allons construire.

### Création de u-boot.bin

Nous téléchargeons U-Boot puis le compilons:

    $ cd $HOME/workspace/xvisor-tree/u-boot
    $ wget ftp://ftp.denx.de/pub/u-boot/u-boot-2016.09-rc1.tar.bz2
    $ tar xvf u-boot-2016.09-rc1.tar.bz2

    $ cd $HOME/workspace/xvisor-tree/u-boot/u-boot-2016.09-rc1
    $ make rpi_3_defconfig
    $ make all

Nous obtenons un fichier **u-boot.bin** qu’il faut copier sur la partition **boot** de la carte sd.

Insérer cette dernière, puis :

    $ cp u-boot.bin /media/<user>/boot

Éditer le fichier config.txt se trouvant sur la partition boot de la carte sd et y ajouter les lignes suivantes :

    enable_uart=1 arm_control=0x200 kernel=u-boot.bin

Ces lignes permettent, respectivement, de :

* activer la console série, option « enable_uart »;
* modifier le mode de démarrage de la carte sur l’architecture aarch64 (mode 64bits), option « arm_control »;
* indiquer le nom de l’exécutable à lancer pour le boot de la carte, ici il s’agit de **u-boot.bin**, option « kernel ».

Il faut démonter le périphérique:

    $ umount /dev/<votre media>

Nous avons également construit dans le répertoire **tools** l’utilitaire **mkimage** qui nous servira par la suite.

A ce point, nous pouvons d’ores et déjà tester si la carte démarre correctement. Nous utilisons un adaptateur USB-TTL, par exemple [https://www.kubii.fr/composants-raspberry-pi/1761-cable-usb-vers-ttl-4-pin-3272496006263.html.](https://www.kubii.fr/composants-raspberry-pi/1761-cable-usb-vers-ttl-4-pin-3272496006263.html)

La connexion est simple :

* pin2 : alimentation câble rouge;
* pin6 : masse, câble noir;
* pin8 : TXD, câble blanc;
* pin 10 : RXD, câble vert.

Pour un exemple, on peut consulter [https://learn.adafruit.com/adafruits-raspberry-pi-lesson-5-using-a-console-cable/overview.](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-5-using-a-console-cable/overview)

On connecte le câble USB, puis on démarre minicom :

    $ minicom -b 115200 -D /dev/ttyUSB0

Dès la mise sous tension de la carte, u-boot attend quelques secondes puis démarre son autoboot. Pour éviter ceci, il suffit d’appuyer sur une touche avant la fin du compte à rebours. Débrancher et reconnecter le câble usb pour redémarrer la carte.

Si vous voyez l’invite de commande U-Boot, alors la carte est fonctionnelle.

### Création de uvmm.bin et udisk.img

Nous modifions l’image vmm.bin pour que U-Boot puisse la charger:

    $ cd $HOME/workspace/xvisor-tree/xvisor
    $ $HOME/workspace/xvisor-tree/u-boot/u-boot-2016.09-rc1/tools/mkimage -A arm64 -O linux -T kernel -C none -a 0x00080000 -e 0x00080000 -n Xvisor -d build/vmm.bin build/uvmm.bin

Même chose pour l’image disk.img:

    $ $HOME/workspace/xvisor-tree/u-boot/u-boot-2016.09-rc1/tools/mkimage -A arm64 -O linux -T ramdisk -a 0x00000000 -n "Xvisor Ramdisk" -d build/disk.img build/udisk.img

Nous avons créé tout ce qu’il faut pour démarrer Xvisor.

Il faut copier tous les fichiers sur la partition **data** de la carte sd :

    $ cp -f $HOME/workspace/xvisor-tree/xvisor/build/uvmm.bin /media/<user>/data
    $ cp -f $HOME/workspace/xvisor-tree/xvisor/build/arch/arm/board/generic/dts/bcm2837/one_guest_virt-v8.dtb /media/<user>/data
    $ cp -f $HOME/workspace/xvisor-tree/xvisor/build/udisk.img /media/<user>/data
    $ cp -f $HOME/workspace/xvisor-tree/xvisor/boot.scr /media/<user>/boot

On démonte la carte puis on démarre la Raspberry Pi 3 avec un terminal connecté.

Après avoir appuyé sur une touche nous nous retrouvons sous l’invite de commande U-Boot.

Pour nous faciliter la vie nous configurons U-Boot de telle sorte que les démarrages suivants soient plus simples.

Sous l’invite de commande U-Boot, saisissez les commandes suivantes :

    U-Boot> setenv bootdelay -1
    U-Boot> setenv xvisor "mmc dev 0:0; ext4load mmc 0:2 0x200000 uvmm.bin; ext4load mmc 0:2 0x800000 one_guest_virt-v8.dtb; ext4load mmc 0:2 0x2000000 udisk.img; bootm 0x200000 0x2000000 0x800000"
    U-Boot> saveenv

Débrancher puis rebranchez le câble USB. On se retrouve sous l’invite de commande U-Boot directement.

Il suffit alors de lancer Xvisor avec :

    U-Boot> run xvisor

Maintenant nous pouvons jouer avec Xvisor.

# Utilisation de Xvisor

Tout d’abord on lance l’invité :

    XVisor# guest kick guest0

Nous lions la sortie série de l’invité à notre console :

    XVisor# vserial bind guest0/uart0
    [guest0/uart0] Virt-v8 Basic Firmware
    [guest0/uart0]
    [guest0/uart0] autoboot: disabled
    [guest0/uart0]
    [guest0/uart0] basic

Nous nous retrouvons dans la console de l’invité. Cet invité permet de jouer avec quelques commandes; ‘hi’, ‘help’, et surtout la commande ‘autoexec’ qui nous permet d’exécuter un ensemble de commandes puis de sauter à une adresse préconfigurée.

A tout moment il est possible de revenir sous l’invite Xvisor avec la séquence ‘ESC+q+x’.

Pour revenir vers l’invité, il suffit de lier sa sortie série à notre console (voir ci-dessus).

Les commandes disponibles sous Xvisor sont assez complètes. La liste est disponible avec ‘help’.

Par exemple la liste des vcpu disponibles : ‘vcpu list’, on voit ce qui est attribué à chaque véritable cpu, leurs états, l’état de notre invité.

On peut consulter la liste des modules chargés ‘module list’, etc.

Revenons à notre invité puis lançons Linux; la commande ‘autoexec’ charge Linux en RAM et démarre Linux :

    XVisor# vserial bind guest0/uart0
    XVisor# autoexec
    [guest0/uart0] / # ls
    [guest0/uart0] bin etc linuxrc root sys
    [guest0/uart0] dev init proc sbin usr
    [guest0/uart0] / #

# Conclusion

Xvisor est un hyperviseur maintenu par une toute petite communauté active. Il tourne sur la majorité des architectures ARM.

Il manque à cet hyperviseur tout un panel de drivers permettant de réaliser des prototypes concrets; il faudrait implémenter les drivers gpio, usb/ethernet, etc. Les développeurs de Xvisor portent les drivers disponibles sous U-Boot et Linux, et les émulateurs sont portés de Qemu.

Le pas à franchir pour non initié est assez important, mais fouiller dans le code de Xvisor permet d’aborder des notions techniques pointues sur la virtualisation, la gestion de la mémoire, la protection mémoire, etc.

Le forum est hébergé sur [https://groups.google.com/forum/#!forum/xvisor-devel](https://groups.google.com/forum/#!forum/xvisor-devel)

Les prochaines étapes consisteront à utiliser deux invités, jouer avec les environnements (Linux), puis développer un driver sous Xvisor.

# Références

Xvisor :

* [http://xhypervisor.org/](http://xhypervisor.org/)
* [https://github.com/xvisor/xvisor/tree/v0.2.9](https://github.com/xvisor/xvisor/tree/v0.2.9)
* [https://github.com/avpatel/xvisor-next](https://github.com/avpatel/xvisor-next)
* ARM : [Fixed Virtual Platforms](https://developer.arm.com/products/system-design/fixed-virtual-platforms)
* Linaro : [https://www.linaro.org/](https://www.linaro.org/)
* U-Boot : [http://www.denx.de/wiki/U-Boot](http://www.denx.de/wiki/U-Boot)
* Busybox : [https://www.busybox.net/](https://www.busybox.net/)

_Originally published at_ [_www.linuxembedded.fr_](http://www.linuxembedded.fr/2017/03/intro_xvisor/) _on March 13, 2017._

# That’s all folks!

Did you enjoy it? If so don’t hesitate to 👏 our article or s[ubscribe to our Innovation watch n](https://www.getrevue.co/profile/smileinnovation)ewsletter!
You can follow Smile on F[acebook,](https://www.facebook.com/smileopensource) T[witter ](https://www.twitter.com/GroupeSmile)& Y[outube.](http://www.youtube.com/user/SmileOpenSource)
