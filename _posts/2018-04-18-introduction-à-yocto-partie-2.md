---
layout: post
url: https://medium.com/@/38d979843cb7
title: Introduction à Yocto (partie 2)
subtitle: Dans un premier article nous avons introduit les principes de Yocto et décrit comment produire une image de test pour des cibles QEMU (x86)…
slug: introduction-à-yocto-partie-2
description: 
tags: embedded-systems,linux,iot,connected-devices
author: Pierre FICHEUX
username: pierre.ficheux
---

# Introduction à Yocto (partie 2)

![](/assets/images/posts//images/posts/images/posts/1*gqaK0--azExuARp3R4LtgA.jpeg)

*Dans [un premier article](https://medium.com/smileinnovation/introduction-%C3%A0-yocto-db56a550ae51) nous avons introduit les principes de Yocto et décrit comment produire une image de test pour des cibles QEMU (x86) et Raspberry Pi 3. Dans ce deuxième article nous allons nous attacher à l’ajout puis à la création de « recettes » permettant d’étendre les possibilités de l’image produite. La lecture du premier article est bien entendu nécessaire à la compréhension de ce deuxième volet.*

# Étendre l’image produite

Nous avions vu que la méthode de production de l’image était la même quelle que soit la cible choisie (dans notre cas QEMU/x86 et Raspberry Pi 3). Nous avions choisi de construire l’image la plus simple soit « core-image-minimal » par les commandes suivantes dans le cas de la Pi 3.

```
$ cd <Yocto-src-path>
$ source oe-init-build-env rpi3-build
$ bitbake core-image-minimal
```

Le test de l’image dépend de la cible choisie puisque un test QEMU/x86 se résume à l’appel de l’émulateur (soit `runqemu qemux86`) alors qu’un test sur cible réelle nécessite de copier l’image produite sur une mémoire flash (ou un disque dur). Dans le cas de la Raspberry Pi 3, nous pouvons créé une Micro-SD par les commandes suivantes.

```
$ umount /dev/mmcblk0p*
$ sudo dd if=tmp/deploy/image/raspberrypi3/core-image-minimal-raspberrypi3.rpi-sdimg of=/dev/mmcblk0
```

L’image actuelle est réduite au strict minimum car elle n’inclut pas les modules du noyau Linux et ne dispose pas de système de gestion de paquets. De ce fait la taille de l’image produite est très faible (56 Mo pour l’image complète).

```
$ ls -lLh tmp/deploy/images/raspberrypi3/core-image-minimal-raspberrypi3.rpi-sdimg 
-rw-r — r — 1 pierre pierre 56M avril 11 15:25 tmp/deploy/images/raspberrypi3/core-image-minimal-raspberrypi3.rpi-sdimg
```

Il est donc judicieux d’étendre l’image en ajoutant d’autres paquets ou fonctionnalités. Dans un premier temps il est tout à fait possible de réaliser cela en modifiant le fichier `conf/local.conf`. A titre d’exemple nous pouvons ajouter un serveur SSH nommé Dropbear en ajoutant la ligne suivante au fichier `conf/local.conf` . Cette ligne indique d’ajouter à l’image finale le résultat de la construction de la recette correspondante. Cette recette est déjà fournie par Yocto dans le répertoire `meta/recipes-core/dropbear`.

```
IMAGE_INSTALL_append = “ dropbear”
```

Nous avions vu que la construction de la recette passait par différentes étapes en partant du téléchargement des sources jusqu’à la création du paquet binaire. Notre image ne disposant pas de système de gestion de paquets, le nouveau composant est ajouté à l’image de manière statique (comme on le ferait avec Buildroot). Le paquet est cependant créé sur le poste de développement comme l’indique la commande suivante.

```
$ ls -l tmp/deploy/ipk/cortexa7hf-neon-vfpv4/dropbear*
-rw-r — r — 2 pierre pierre 119004 avril 11 15:38 tmp/deploy/ipk/cortexa7hf-neon-vfpv4/dropbear_2017.75-r0_cortexa7hf-neon-vfpv4.ipk
-rw-r — r — 2 pierre pierre 881666 avril 11 15:38 tmp/deploy/ipk/cortexa7hf-neon-vfpv4/dropbear-dbg_2017.75-r0_cortexa7hf-neon-vfpv4.ipk
-rw-r — r — 2 pierre pierre 832 avril 11 15:38 tmp/deploy/ipk/cortexa7hf-neon-vfpv4/dropbear-dev_2017.75-r0_cortexa7hf-neon-vfpv4.ipk
```

Nous verrons plus tard pourquoi il existe trois paquets binaires pour une seule recette (soit des paquets `-dbg` et `-dev` en plus du paquet principal). La commande suivante indique quels sont les composants installés par le paquet binaire. Le format IPK étant proche du format DEB utilisé sur Debian/Ubuntu, le contenu du paquet est visible grâce à la commande `dpkg`.

```
$ dpkg -c tmp/deploy/ipk/cortexa7hf-neon-vfpv4/dropbear_2017.75-r0_cortexa7hf-neon-vfpv4.ipk 
drwxrwxrwx root/root 0 2018–04–11 15:38 ./
drwxr-xr-x root/root 0 2018–04–11 15:38 ./etc/
drwxr-xr-x root/root 0 2018–04–11 15:38 ./etc/default/
drwxr-xr-x root/root 0 2018–04–11 15:38 ./etc/dropbear/
drwxr-xr-x root/root 0 2018–04–11 15:38 ./etc/init.d/
-rwxr-xr-x root/root 2168 2018–04–11 15:38 ./etc/init.d/dropbear
drwxr-xr-x root/root 0 2018–04–11 15:38 ./var/
drwxr-xr-x root/root 0 2018–04–11 15:38 ./usr/
drwxr-xr-x root/root 0 2018–04–11 15:38 ./usr/sbin/
-rwxr-xr-x root/root 224168 2018–04–11 15:38 ./usr/sbin/dropbearmulti
lrwxrwxrwx root/root 0 2018–04–11 15:38 ./usr/sbin/dropbearconvert -> ./dropbearmulti
lrwxrwxrwx root/root 0 2018–04–11 15:38 ./usr/sbin/dropbear -> ./dropbearmulti
lrwxrwxrwx root/root 0 2018–04–11 15:38 ./usr/sbin/dropbearkey -> ./dropbearmulti
drwxr-xr-x root/root 0 2018–04–11 15:38 ./usr/bin/
lrwxrwxrwx root/root 0 2018–04–11 15:38 ./usr/bin/dbclient -> /usr/sbin/dropbearmulti
```

Au démarrage de la nouvelle image, nous constatons l’initialisation et le démarrage de Dropbear.

```
Starting Dropbear SSH server: Generating key, this may take a while… 
```

Outre l’ajout de recettes, nous avions vu dans le premier article qu’il était possible d’ajouter des « features » comme la prise en compte des paquets binaires. Cette fonctionnalité est indispensable si nous voulons tester le développement et l’ajout dynamique de recette. 
Pour ce faire nous ajoutons la ligne suivante au fichier `conf/local.conf` et nous construisons de nouveau l’image. Notons que nous avons commenté la ligne concernant le serveur Dropbear car nous l’ajouterons a posteriori en utilisant le système de gestion de paquets. Notons également qu’il est nécessaire de spécifier le format IPK car Yocto utilise par défaut le format RPM. Nous avons également augmenté la taille du root-filesystem de 50 Mo afin de pouvoir manipuler quelques paquets binaires.

```
EXTRA_IMAGE_FEATURES = “package-management”
PACKAGE_CLASSES = “package_ipk”
IMAGE_ROOTFS_EXTRA_SPACE = “50000”
#IMAGE_INSTALL_append = “ dropbear”
```

Dans la suite de l’article nous partirons du principe que l’image utilise cette configuration. Au démarrage du système, celui-ci doit disposer de la commande `opkg`.

```
root@raspberrypi3:~# type opkg 
opkg is a tracked alias for /usr/bin/opkg 
```

# Configurer le gestionnaire de paquets

Le gestionnaire de paquets utilise la base des paquets binaires présente sur le poste de développement dans le répertoire `tmp/deploy/ipk`. Comme nous l’avions vu dans le premier article, ce répertoire contient trois sous-répertoires correspondant aux trois types de paquets (indépendant de l’architecture, spécifique à à l’architecture, spécifique à la cible).

```
$ ls -l tmp/deploy/ipk/
total 568
drwxr-xr-x 2 pierre pierre 4096 avril 11 16:33 all
drwxr-xr-x 2 pierre pierre 372736 avril 11 16:33 cortexa7hf-neon-vfpv4
drwxr-xr-x 2 pierre pierre 196608 avril 11 16:33 raspberrypi3
```

En premier lieu on doit construire l’index à partir de la liste des paquets.

```
$ bitbake package-index
```

On peut alors donner accès au répertoire par le protocole HTTP. Dans le cas de notre test, on peut utiliser le mini-serveur HTTP intégré à Python qui utilise le port 8000.

```
$ cd tmp/deploy/ipk/
$ python -m SimpleHTTPServer
Serving HTTP on 0.0.0.0 port 8000 …
```

Du coté de la cible on doit modifier le fichier* *`/etc/opkg/opkg.conf`* *afin d’ajouter les caractéristiques du serveur. En supposant que l’adresse IP du PC de développement est 192.168.1.23, on ajoute les lignes suivantes au fichier.

```
src/gz all http://192.168.1.23:8000/all 
src/gz cortexa7hf-neon-vfpv4 http://192.168.1.23:8000/cortexa7hf-neon-vfpv4 
src/gz raspberrypi3 http://192.168.1.23:8000/raspberrypi3 
```

On peut alors mettre à jour l’index des paquets sur la cible.

```
root@raspberrypi3:~# opkg update
```

Cette commande a pour effet d’afficher les traces suivantes dans la fenêtre du serveur HTTP, l’adresse IP de la carte étant 192.168.1.16.

```
192.168.1.16 - - [11/Apr/2018 16:34:18] "GET /all/Packages.gz HTTP/1.1" 200 -
192.168.1.16 - - [11/Apr/2018 16:34:18] "GET /cortexa7hf-neon-vfpv4/Packages.gz HTTP/1.1" 200 -
192.168.1.16 - - [11/Apr/2018 16:34:18] "GET /raspberrypi3/Packages.gz HTTP/1.1" 200 -
```

On peut alors utiliser quelques options de la commande `opkg`. En premier lieu on peut obtenir la liste des paquets installés.

```
root@raspberrypi3:~# opkg list-installed
```

On peut ensuite afficher le contenu d’un paquet particulier.

```
root@raspberrypi3:~# opkg files busybox 
Package busybox (1.24.1-r0) is installed on root and has the following files: 
/bin/sh 
/bin/busybox.suid 
/bin/busybox 
/etc/busybox.links.nosuid 
/etc/busybox.links.suid 
/bin/busybox.nosuid
```

On peut également rechercher le paquet Dropbear et l’installer dynamiquement sur la cible. Le paquet est configuré lors de l’installation.

```
root@raspberrypi3:~# opkg list | grep dropbear 
dropbear — 2017.75-r0 — A lightweight SSH and SCP implementation 
dropbear-dbg — 2017.75-r0 — A lightweight SSH and SCP implementation — Debugging files 
dropbear-dev — 2017.75-r0 — A lightweight SSH and SCP implementation — Development files
```

```
root@raspberrypi3:~# opkg install dropbear 
Installing dropbear (2017.75) on root 
Downloading http://192.168.1.23:8000/cortexa7hf-neon-vfpv4/dropbear_2017.75-r0_c
ortexa7hf-neon-vfpv4.ipk
```

# Créer des recettes

Nous pouvons maintenant nous attacher à créer quelques recettes simples que nous pourrons ajouter à notre environnement Yocto puis à notre cible. Une recette est un fichier* *`.bb` (pour *BitBake*) utilisant un syntaxe décrivant des métadonnées. Les directives de la recette sont exécutées par la commande `bitbake`. La notion de recette est utilisée quelle que soit la complexité du composant (un simple exemple « Hello World », l’outil BusyBox, le noyau Linux et même l’image du système).

```
$ cd meta
$ ls recipes-core/images
build-appliance-image core-image-minimal-dev.bb
build-appliance-image_15.0.0.bb core-image-minimal-initramfs.bb
core-image-base.bb core-image-minimal-mtdutils.bb
core-image-minimal.bb core-image-tiny-initramfs.bb
recipes-core/images/core-image-minimal.bb
```

```
$ ls recipes-core/busybox
busybox busybox-1.24.1 busybox_1.24.1.bb busybox.inc files
```

La recette du noyau est fournie par la couche (layer) BSP externe à Yocto et disponible auprès du fabricant de la carte ou d’une communauté de développement (comme c’est le cas pour la Raspberry Pi).

```
$ cd ../meta-raspberrypi
$ ls recipes-kernel/linux
linux-raspberrypi_4.9.bb linux-raspberrypi-dev.bb linux-raspberrypi.inc
```

Comme nous pouvons le constater, une recette est toujours associée à un layer (`meta`, `meta-raspberrypi`, `meta-<nom-de-layer)` dans un sous-répertoire de type `recipes-*/<nom-de-recette>`.

### Les différents type de recettes

Nous pouvons classifier (grossièrement) les recettes en quelques types bien identifiés. Ces types sont déduits des outils utilisés pour la production des projets libres dans un environnement de type UNIX/Linux (même si ces outils existent également pour d’autres systèmes d’exploitation).

* Utilisation de l’outil Autotools³

* Utilisation de l’outil CMake⁴

* Plus rarement, utilisation de la commande `make` et d’un fichier `Makefile`

Il n’est bien entendu pas envisageable de décrire les outils Autotools et CMake dans le cadre de cet article et nous renvoyons donc le lecteur aux entrées de bibliographie. Ces outils sont prévus pour permettre la compilation (croisée ou non) d’un projet sur (et à destination de) différents types de systèmes. L’utilisation d’un simple `Makefile` est un peu « démodée » car ce dernier ne prend pas automatiquement en compte les caractéristiques des systèmes comme le font Autotools et CMake.
Yocto permet de créer des recettes sans réellement se préoccuper du fonctionnement de ces outils en utilisant la notions de classe. Les principales classes disponibles sont localisées dans le répertoire `meta/classes` sous la forme de fichiers `.class`. A titre d’exemple, le fichier suivant correspond à la classe permettant de traiter les sources basées sur Autotools.

```
$ ls -l meta/classes/autotools.bbclass 
-rw-rw-r — 1 pierre pierre 8798 févr. 9 22:25 meta/classes/autotools.bbclass 
```

Comme nous le verrons dans les exemples qui suivent, on peut utiliser une classe en invoquant simplement la directive `inherti <nom-de-classe>`dans le fichier de recette.

### Un exemple (très) simple

Avant de décrire le format d’une recette nous allons utiliser l’outil `yocto-layer`* *fourni par Yocto afin de créer un layer de test `meta-exampl`. Outre la création du layer, `yocto-layer` permet d’ajouter automatiquement une recette de test au layer créé. Pour cela on utilise la commande suivante à partir du répertoire des sources de Yocto (ou même dans un répertoire quelconque).

```
$ cd <Yocto-src-path>
$ yocto-layer create example
```

```
Please enter the layer priority you’d like to use for the layer: [default: 6] 
Would you like to have an example recipe created? (y/n) [default: n] y
Please enter the name you’d like to use for your example recipe: [default: example] 
Would you like to have an example bbappend file created? (y/n) [default: n]
```

```
New layer created in meta-example.
```

```
Don’t forget to add it to your BBLAYERS (for details see meta-example/README).
```

Une nouvelle recette nommée *example* est créée dans l’arborescence. La recette est constituée du fichier `.bb` ainsi que d’un répertoire contenant les données (dans notre cas les sources de l’exemple).

```
meta-example/
├── conf
│   └── layer.conf
├── COPYING.MIT
├── README
└── recipes-example
    └── example
        ├── example-0.1
        │   ├── example.patch
        │   └── helloworld.c
        └── example_0.1.bb
```

Comme l’indique le dernier message produit par `yocto-layer`, on doit ajouter le nouveau layer à l’environnement de compilation. Cette action est réalisée par la commande `bitbake-layers`.

```
$ cd rpi3-build 
$ bitbake-layers add-layer ../meta-example
```

Cette commande ajoute simplement le chemin d’accès du layer au fichier `conf/bblayers.conf`. On peut alors construire la recette (i.e. produire le paquet binaire).

```
$ bitbake example
Les paquets sont disponibles dans le répertoire prévu.
$ ls -l tmp/deploy/ipk/cortexa7hf-neon-vfpv4/example*
-rw-r — r — 2 pierre pierre 2176 avril 11 18:27 tmp/deploy/ipk/cortexa7hf-neon-vfpv4/example_0.1-r0_cortexa7hf-neon-vfpv4.ipk
-rw-r — r — 2 pierre pierre 3960 avril 11 18:27 tmp/deploy/ipk/cortexa7hf-neon-vfpv4/example-dbg_0.1-r0_cortexa7hf-neon-vfpv4.ipk
-rw-r — r — 2 pierre pierre 732 avril 11 18:27 tmp/deploy/ipk/cortexa7hf-neon-vfpv4/example-dev_0.1-r0_cortexa7hf-neon-vfpv4.ipk
```

Le paquet « principal » contient l’exécutable `helloworld` issu de la compilation du fichier* *`helloworld.c`.

```
$ dpkg -c tmp/deploy/ipk/cortexa7hf-neon-vfpv4/example_0.1-r0_cortexa7hf-neon-vfpv4.ipk 
drwxrwxrwx root/root 0 2018–04–11 18:27 ./
drwxr-xr-x root/root 0 2018–04–11 18:27 ./usr/
drwxr-xr-x root/root 0 2018–04–11 18:27 ./usr/bin/
-rwxr-xr-x root/root 5552 2018–04–11 18:27 ./usr/bin/helloworld
```

Le paquet `-dbg`contient une version de l’exécutable « débogable » sur la cible (en utilisant `gdbserver`). Le paquet `-dev`est utile dans le cas des recettes de bibliothèques pour lesquelles il est nécessaire de disposer du fichier binaire (suffixé `.so`) sur la cible mais également dans l’environnement de développement croisé (SDK) produit par Yocto. Dans le cas d’un exécutable, ce paquet est bien entendu vide.
Avant d’installer le nouveau paquet sur la cible on doit mettre à jour l’index sur le PC de développement.

```
$ bitbake package-index
```

On doit également effectuer la mise à jour (update) de la liste sur la cible.

```
root@raspberrypi3:~# opkg update 
Downloading http://192.168.1.23:8000/all/Packages.gz. 
Updated source ‘all’. 
Downloading http://192.168.1.23:8000/cortexa7hf-neon-vfpv4/Packages.gz. 
Updated source ‘cortexa7hf-neon-vfpv4’. 
Downloading http://192.168.1.23:8000/raspberrypi3/Packages.gz. 
Updated source ‘raspberrypi3’.
```

On peut alors installer et tester le nouveau paquet.

```
root@raspberrypi3:~# opkg install example 
Installing example (0.1) on root 
Downloading http://192.168.1.23:8000/cortexa7hf-neon-vfpv4/example_0.1-r0_cortex
a7hf-neon-vfpv4.ipk. 
Configuring example.
```

```
root@raspberrypi3:~# helloworld 
Hello World!
```

### Syntaxe du fichier de recette

Après avoir installé le paquet nous allons décrire rapidement le format du fichier de recette. Les premières lignes décrivent le composant ainsi que la catégorie. Ces informations seront utilisées par le gestionnaire de paquets.

```
SUMMARY = “Simple helloworld application”
SECTION = “examples”
```

On déclare ensuite la licence des sources que l’on authentifie grâce à un checksum.

```
LICENSE = “MIT”
LIC_FILES_CHKSUM = “file://${COMMON_LICENSE_DIR}/MIT;md5=0835ade698e0bcf8506ecda2f7b4f302”
```

La variable* *`SRC_URI` doit contenir la liste de tous les composants nécessaires à la construction du paquet. Dans notre cas la liste se limite à un fichier source local (d’où l’opérateur `file://`) présent dans le sous-répertoire `example_0.1`.

```
SRC_URI = “file://helloworld.c”
```

Les lignes suivantes décrivent la compilation du fichier source et l’installation de l’exécutable. Le fichier `Makefile` étant absent on doit décrire précisément comment réaliser ces opérations en invoquant les commandes nécessaires.

```
S = “${WORKDIR}”
```

```
do_compile() {
 ${CC} ${LDFLAGS} helloworld.c -o helloworld
}
```

```
do_install() {
 install -d ${D}${bindir}
 install -m 0755 helloworld ${D}${bindir}
}
```

### Recettes Autotools et CMake

Dans la réalité la plupart des projets utilisent Autotools, CMake ou plus rarement un simple fichier `Makefile`. Dans cette section nous allons voir comment écrire une recette en utilisant les classes Autotools et CMake, ce qui facilite grandement la tâche. 
Considérons un exemple « Hello World » proche du précédent mais basé sur Autotools. Le contenu du fichier de recette est décrit ci-dessous.

```
DESCRIPTION = “Helloworld software (autotools)”LICENSE = “GPLv2”
LIC_FILES_CHKSUM = “file://COPYING;md5=8ca43cbc842c2336e835926c2166c28b”
```

```
SRC_URI = “http://pficheux.free.fr/pub/tmp/mypack-auto-1.0.tar.gz"
```

```
inherit autotools
```

```
SRC_URI[md5sum] = “b282082e4e5cc8634b7c6caa822ce440”
```

La première différence avec le premier exemple est l’obtention des sources auprès d’un serveur externe (ce qui est le cas le plus fréquent). A partir du moment ou une archive externe est utilisée, on doit déclarer le checksum du fichier comme décrit dans la dernière ligne de la recette. Le traitement des sources est entièrement pris en charge par la classe et il suffit donc d’utiliser la directive `inherit`. 
Le cas d’une recette basée sur CMake est identique en utilisant la classe correspondante.

```
DESCRIPTION = “Helloworld software (CMake)”
LICENSE = “GPLv2”
LIC_FILES_CHKSUM = “file://COPYING;md5=8ca43cbc842c2336e835926c2166c28b”
PR = “r0”
SRC_URI = “http://pficheux.free.fr/pub/tmp/mypack-cmake-1.0.tar.gz"
```

```
inherit cmake
```

```
SRC_URI[md5sum] = “70e89c6e3bff196b4634aeb5870ddb61”
```

# Conclusion

Ce deuxième article nous a permis d’effleurer les techniques de création des recettes Yocto ainsi que leur intégration dans une image. Le lecteur désirant aller plus loin pourra consulter l’ouvrage de l’auteur⁵ ou bien d’autres ouvrages dédiés à Yocto⁶.

# Bibliographie

[*[1] Projet Yocto https://www.yoctoproject.org/
](https://www.yoctoproject.org/)[[2] Manuel de référence Yocto http://www.yoctoproject.org/docs/2.4.1/ref-manual/ref-manual.html](http://www.yoctoproject.org/docs/2.4.1/ref-manual/ref-manual.html)
[[3] Autotools tutorial (LRDE) https://www.lrde.epita.fr/~adl/autotools.html](https://www.lrde.epita.fr/~adl/autotools.html)
[[4] Tutoriel CMake http://sirien.metz.supelec.fr/depot/SIR/TutorielCMake/index.html](http://sirien.metz.supelec.fr/depot/SIR/TutorielCMake/index.html)
[[5] « Linux embarqué, mise en place et développment » (P Ficheux) https://www.eyrolles.com/Informatique/Livre/linux-embarque-978221267484](https://www.eyrolles.com/Informatique/Livre/linux-embarque-978221267484)
[[6] Quelques ouvrages dédiés à Yocto https://www.yoctoproject.org/learn/books*](https://www.yoctoproject.org/learn/books)


