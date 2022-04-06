---
layout: post
url: https://medium.com/@/db56a550ae51
title: "Introduction à Yocto (partie 1)"
subtitle: "Yocto est devenu un standard de l’industrie pour la technologie « Linux embarqué ». Dans cette courte série de deux articles nous décrirons…"
slug: introduction-à-yocto
description:
tags:
- embedded-systems
- linux
- iot
- connected-devices
author: pific
---

![](/assets/images/posts/0*3oFSYv3ykuGC1Gzi.jpgjpeg)

*Yocto est devenu un standard de l’industrie pour la technologie « Linux embarqué ». Dans cette courte série de deux articles nous
décrirons tout d’abord les principes de cet outil, puis nous décrirons quelques exemples plus avancés dans un deuxième article.*

### **De la distribution au « build system »**

La plupart des utilisateurs et développeurs GNU/Linux utilisent des « distributions » (Debian, Ubuntu, Fedora, Red Hat, etc.). Une distribution est constituée d’un ensemble de composants rassemblés dans des « paquets » (packages). Ces paquets sont installés grâce à une procédure (graphique) la plus simple possible, l’utilisateur final n’étant pas forcément un développeur. Dans de nombreux cas on installe la distribution sur un PC/x86 afin de créer un poste de travail qui peut aller de la simple bureautique (Internet, LibreOffice) au développement C/C++, Python ou Java en intégrant des outils comme Eclipse. Un autre cas fréquent est l’installation d’un serveur. Grâce à une autre interface graphique on peut ajouter, retirer ou mettre à jour les paquets installés sur la distribution.

L’espace occupé par une distribution classique est relativement important car il est rare qu’un disque dur récent ait une capacité inférieure à 1 To (voire plus). Suivant les composants installés, la distribution utilisera donc plusieurs Go, voire plusieurs dizaines de Go sur le disque, et sera composée de milliers de paquets (plus de 3000 sur mon poste de travail sous Ubuntu).

Depuis plusieurs années GNU/Linux est utilisé pour des solutions industrielles, ou plus récemment pour des solutions « embarquées ». La principale différence correspondant à l’empreinte mémoire utilisée ainsi que la puissance du matériel (CPU, RAM, stockage). A titre d’exemple, la quasi-totalité des boîtiers « gateway » et décodeurs TV proposés par les fournisseurs d’accès à Internet (FAI) utilisent GNU/Linux dans une version adaptée. Il en est de même pour les décodeurs satellite. Ces produits étant largement diffusés (plusieurs millions d’exemplaires) il est indispensable de réduire le coût du matériel et donc en premier lieu l’empreinte mémoire du système d’exploitation donc la taille de la mémoire flash hébergeant ce système (ainsi que la RAM). En outre, ces produits doivent être d’une grande fiabilité. Il est donc indispensable de maîtriser la production du système ainsi que la mise à jour. Pour toutes ces raisons, l’utilisation d’une distribution classique n’est pas recommandée et l’on créera l’image du système à partir des sources des composants en utilisant un outil dédié nommé « build system » que l’on peut traduire par « outil de construction ».

### **Les différents outils disponibles**

Pendant plusieurs années il n’existait pas d’outils standards et ceux d’aujourd’hui ont été initialement créés en tant qu’outils internes pour d’autres projets. L’outil [Buildroot](https://buildroot.org)¹ était au départ utilisé pour le projet uClibc (puis uClibc-ng). Buildroot a toujours à ce jour une approche « statique » ce qui signifie qu’il n’est pas possible d’ajouter/retirer/mettre à jour des composants sur la cible installée. De même, l’intégralité des recettes et des BSP (Board Support Package) disponibles sous Buildroot sont fournis dans le dépôt officiel (soit près de 1800 recettes et 190 cibles matérielles).

Son principal concurrent, [OpenEmbedded](http://www.openembedded.org/wiki/Main_Page)²— à l’origine de Yocto — fut démarré dans le projet OpenZaurus, une version de GNU/Linux pour le PDA Zaurus de Sharp (sorti en 2002). OpenEmbedded permet de créer — si on le désire — une véritable « distribution » sur mesure bénéficiant d’un système de gestion de paquets similaire à celui des distributions classiques. Nous verrons également que son approche est beaucoup plus « dynamique » tant au niveau des recettes que des BSP disponibles.

Cette liste des outils n’est pas exhaustive car d’autres produits similaires sont encore utilisés, citons [OpenWrt](https://openwrt.org/)⁴ initialement développé pour le routeur WRT54G de Linksys et toujours utilisé sur des passerelles grand public, [PTXdist](https://www.pengutronix.de/en/software/ptxdist.html)⁵ créé par Pengutronix ou bien l’ancêtre [LTIB](http://ltib.org/)⁶ autrefois utilisé pour les BSP fournis par Freescale (à l’époque). Ces trois derniers outils sont très similaires à Buildroot malgré quelques différences comme le système de gestion de paquets disponible sur OpenWrt. Le principe de fonctionnement d’un build system est toujours le même :

• L’outil fournit des fichiers décrivant la manière de produire un composant binaire à partir de ses sources pour une cible matérielle donnée (on parle de « recette » similaire à une recette de cuisine). Il faut noter que l’outil ne fournit pas les sources des composants et qu’on les obtient à partir des dépôts des différents projets (kernel.org, busybox.net, github.com, etc.).

• A partir des composants sélectionnés, l’outil produit les images du système (bootloader, noyau Linux, root-filesystem) et souvent une image unique à écrire sur la mémoire flash de la cible (ou bien la Micro-SD).

![](/assets/images/posts/1*E3hJhQ-nRpy-U7m4MabpMQ.png)

Les outils similaires à Buildroot ont en commun la possibilité de définir le contenu de l’image à produire (i.e. les composants à intégrer) en utilisant un outil graphique identique à celui utilisé pour la configuration du noyau Linux.

![Écran de configuration de Buildroot](/assets/images/posts/1*mOoTMXmm2JIRgQCOhOKgXw.png)

En revanche OpenEmbedded (et donc Yocto) n’utilisent — presque — pas d’outil graphique et l’on définit le contenu de l’image par des fichiers de configuration. Cette approche est plus complexe pour l’utilisateur occasionnel mais permet de créer des configurations plus avancées dans le cas d’une approche industrielle.

### **Introduction à Yocto / OpenEmbedded**

Comme nous l’avons dit OpenEmbedded est né suite aux premiers travaux sur OpenZaurus (qui utilisait Buildroot). Les limites de la version Buildroot de l’époque (2003) — et probablement son approche « statique » — conduisirent les trois principaux développeurs (Chris Larson, Michael Lauer, and Holger Schurig ) à définir leur propre outil de construction constitué de deux projets indépendants.

* Un ensemble de recettes (OpenEmbedded-Core)

* Un outil de construction et d’exploitation des recettes (BitBake)

Le projet était à l’époque assez complexe à utiliser du fait d’un cruel manque de documentation. Son intégration en 2010 au projet Yocto (projet officiel de la fondation Linux) permit de rationaliser l’architecture et de rendre son usage plus accessible. Yocto est un projet « chapeau » intégrant différents projets libres comme OpenEmbedded, BitBake, Poky ainsi que les grands noms de l’industrie tant au niveau du matériel que du logiciel (Intel, TI, Xilinx , AMD, Mentor Graphics, Wind River, etc.). De ce fait, Yocto est aujourd’hui une référence industrielle et il est utilisé par les fabricants de matériel pour fournir leur BSP Linux. Il est également à la base des solutions d’éditeurs de logiciels comme celles de Wind River ([Wind River Linux](https://www.windriver.com/products/linux/)⁷ ). D’autres projets majeurs dans l’informatique embarquée dédiée à l’automobile (IVI pour In Vehicle Infotainment) comme [GENIVI](https://www.yoctoproject.org/product/genivi-baseline)⁸ ou [AGL](https://www.automotivelinux.org)⁹ sont également basés sur Yocto.

L’approche de Yocto est très différente de celle de Buildroot. En effet, Yocto est basé sur une architecture en couches (layers) permettant de construire l’image d’une distribution de référence nommée Poky et d’enrichir cette image en y ajoutant d’autres couches (donc de nouvelles recettes). Seules les couches indispensables (soit oe-core et meta-yocto) sont fournies par le projet Yocto. On peut ensuite ajouter des couches externes liées au matériel (BSP), à des systèmes graphiques comme Qt ou à des composants métier. Comme nous pouvons le constater dans la liste des [layers répertoriés](http://layers.openembedded.org/layerindex/branch/master/layers)¹⁰ , un layer est le plus souvent nommé meta-<nom-du-layer> et correspond concrètement à une arborescence de sous-répertoires contenant des recettes.

![Les couches (layers) de Yocto](/assets/images/posts/1*YOgkFCLDfklNUZi0oP7aWg.png)

Poky est la distribution de référence mais d’autres sont disponibles comme ELDK de DENX (meta-eldk), Arago de TI (meta-arago-distro), Angstrom (meta-angstrom). Rappelons que des produits commerciaux comme Wind River Linux ou MontaVista Carrier Grade utilisent Yocto comme système de construction.

Yocto produit systématiquement des paquets binaires au format RPM, IPK ou DEB. Basé sur le format DEB, le format IPK (Itsy Package Format) a l’avantage d’être très compact, bien plus que les deux autres formats (RPM et DEB) compatibles avec les distributions classiques. Le format IPK est également utilisé pour OpenWrt déjà cité en début d’article.

La base de données des paquets est le plus souvent installée sur la cible ce qui permet l’ajout/suppression/mise à jour comme sur une distribution classique. Cette fonctionnalité n’est cependant pas activée par défaut sur l’image la plus légère que nous testerons au paragraphe suivant.

### **Un premier test de Yocto**

Un des principes fondamentaux de Yocto est de maintenir une liste de cibles de référence constituée en majorité de cibles émulées par l’outil QEMU (plus quelques cibles génériques, soit 13 au total). Les noms des cibles sont *qemux86, qemuarm, qemumips, *etc. Si l’on désire construire l’image pour une cible réelle, il faudra obtenir le BSP sous forme de layer dans la liste citée en [10]. Le layer peut également être disponible auprès du fournisseur sans être référencé ni public (mais dans ce cas-là, la méfiance est de mise !).

***REMARQUE** : nous avons dit que le développement des layers externes (comme les BSP) n’est pas de la responsabilité du projet Yocto. Il est donc de la responsabilité des mainteneurs du BSP de suivre les évolutions de Yocto. Ce point est à prendre en compte lors du choix d’une cible matérielle car dans le cas contraire l’utilisateur sera limité à des anciennes versions de Yocto et donc limité dans l’utilisation d’autres layers.*

Dans la suite de l’article nous utiliserons le BSP Yocto pour la célèbre carte Raspberry Pi, soit [meta-raspberrypi](http://git.yoctoproject.org/cgit/cgit.cgi/meta-raspberrypi)¹¹ . Paradoxalement, le support Yocto de cette carte « grand public » est de très bonne qualité, bien meilleur que celui de certaines cartes industrielles. Avant cela nous allons construire une image pour la cible par défaut soit qemux86.

### **Test pour QEMU/x86**

La première étape consiste à récupérer les sources du projet Yocto. La branche correspond à la version choisie, chaque version ayant un nom de code (rocko pour 2.4, pyro pour 2.3, morty pour 2.2, etc.).

```
$ git clone -b <branche> git://git.yoctoproject.org/poky
```

On doit alors créer un répertoire de compilation en utilisant le script **oe-init-build-env**. Cette commande a pour effet de créer le répertoire choisi (soit **qemux86-build**) contenant les fichiers **local.conf** et **bblayers.conf** dans le sous-répertoire **conf**.

```
$ cd poky
$ source oe-init-build-env qemux86-build
```

La construction de l’image la plus simple nommée « core-image-minimal » correspond à la ligne suivante :

```
$ bitbake core-image-minimal
```

La création de cette image — bien que légère — est relativement longue. En effet, outre la création de l’image à installer sur la cible, la première compilation produit également les outils de compilation croisés. Sur une machine raisonnablement puissante (i7, 8 Go de RAM), le temps de compilation avoisine une heure même si l’utilisation d’un disque SSD améliore considérablement les performances. Une fois l’image produite, on peut la tester en utilisant la commande suivante :

```
$ runqemu qemux86
```

Cette dernière commande doit conduire à l’affichage de la fenêtre de l’émulateur indiquant le démarrage du système.

![Test de l’image Poky pour QEMU/x86](/assets/images/posts/1*UwrLlrU-gAohRM4v4tODRw.png)

### **Test pour Raspberry Pi 3**

Si l’on désire tester la même image sur une Raspberry Pi 3, il faut obtenir le layer correspondant et indiquer le type de cible en renseignant la variable d’environnement **MACHINE** dans le fichier **local.conf**. Il est important d’indiquer la même branche que pour les sources de Yocto lors du test précédent.

```
$ cd poky
$ git clone -b <branche> git://git.yoctoproject.org/meta-raspberrypi
```

On crée un nouveau répertoire de travail et l’on ajoute le nouveau layer au fichier **bblayers.conf** en utilisant la commande **bitbake-layers** (différente de **bitbake** !). On note que l’on peut utiliser plusieurs répertoires de compilation dans la même arborescence Yocto, ce qui n’est pas possible avec Buildroot.

```
$ source oe-init-build-env rpi3-build
$ bitbake-layers add-layer ../meta-raspberrypi
```

On définit ensuite le type de cible.

```
echo “MACHINE = \”raspberrypi3\”” >> conf/local.conf
```

On peut alors créer l’image.

```
$ bitbake core-image-minimal
```

Pour tester sur la Raspberry Pi 3, on copie l’image produite sur une Micro-SD.

> $ umount /dev/mmcblk0p*
$ sudo dd if=tmp/deploy/image/raspberrypi3/core-image-minimal-raspberrypi3.rpi-sdimg of=/dev/mmcblk0

Lors du test sur la carte on obtient finalement les traces suivantes :

![](/assets/images/posts/1*SaEwv9F3F1AWmsFMb4cTNg.png)

On note la très faible empreinte mémoire du système (5,5 Mo) mais il est vrai que nous n’avons pas installé les systèmes de gestion de paquets (package management) sur l’image ni les modules du noyau. L’ajout du système de gestion de paquets IPK permet cependant de conserver une empreinte mémoire très raisonnable (8,5 Mo).

```
root@raspberrypi3:~# df -h
```

```
Filesystem  Size   Used Available Use% Mounted on
/dev/root   14.5M  8.5M 5.2M      62%  /
```

### **Répertoires produits**

Le test précédent nous a permis de produire une image à tester. Dans cette partie nous allons voir les éléments créés par Yocto (en fait par BitBake). Ces éléments sont tous situés dans des sous-répertoires du répertoire de construction, soit **rpi3-build** dans notre cas. Étrangement, les fichiers directement utilisables sur la cible (en particulier l’image à installer) sont localisés dans un sous-répertoire de **tmp** et dans cet article d’introduction, nous évoquerons uniquement **tmp/deploy** et **tmp/work**. Le plus important est certainement **tmp/deploy/images/raspberrypi3** puisqu’il contient tous les éléments nécessaires au démarrage de la cible (noyau Linux, modules, fichiers « device tree », images du root-filesystem, etc.). Certains BSP, comme celui de la Raspberry Pi, produisent également une image directement utilisable sur une mémoire flash (cas de notre test).

Si l’on détaille un peu plus **tmp/deploy** on constate qu’il contient trois sous-répertoires.

```
tmp/deploy/
I__images
I__ipk
I__licences
```

Le sous-répertoire **licences** contient les licences des composants produits lors de la création de l’image. Ce point est important lors d’une démarche industrielle car il est souvent nécessaire de disposer de cette liste. Si l’on détaille le sous-répertoire **ipk**, on constate qu’il contient lui-même trois sous-répertoires.

```
tmp/deploy/ipk/
I__all
I__cortexa7hf-neon-vfpv4
I__raspberrypi3
```

Le but des sous-répertoires est de classer les paquets binaires produits par catégories. Le sous-répertoire **all** correspond à des paquets indépendants de l’architecture de la cible (scripts, fichiers de données). Le sous-répertoire **cortexa7hf-neon-vfpv4** correspond à l’architecture matérielle de la cible (ici un cœur Cortex A7). Une cible utilisant la même architecture devrait pouvoir utiliser ces paquets. Le sous-répertoire **raspberrypi3** correspond à des paquets spécifiques à la cible comme les pilotes ou les fichiers « device tree » de la Raspberry Pi 3. Notons que si le format de paquets choisi est RPM (valeur par défaut) ou DEB, nous verrons apparaître les sous-répertoires **rpm** et/ou **deb**.

Le répertoire **tmp/work** nous est moins utile dans un premier temps car il correspond à un répertoire de travail hébergeant la production de chaque paquet binaire à partir des recettes. Outre cela il peut contenir des informations intéressantes comme le contenu du root-filesystem ou les traces d’exécution des recettes (ainsi que les erreurs!). Il est donc destiné à un utilisateur avancé développant ses propres recettes comme nous le verrons dans le deuxième article.

### **Configuration avancée**

Pour l’instant nous avons uniquement spécifié le type de cible par la variable **MACHINE** dans **local.conf** ainsi que l’ajout du layer meta-raspberry grâce à la commande **bitbake-layers** qui impacte le fichier **bblayers.conf**. Nous allons terminer ce premier article en indiquant quelques options de configurations que l’on peut mentionner dans **local.conf**. Une description complète des options est bien entendu disponible dans la documentation Yocto, en particulier le [manuel de référence](http://www.yoctoproject.org/docs/2.4.1/ref-manual/ref-manual.html)¹² . Lors du deuxième article nous décrirons plus précisément la notion de « feature » évoquée ci-après.

### **Réduction de l’espace utilisé**

La production d’une image correspond à l’exécution par BitBake de plusieurs milliers de recettes. Par défaut chaque étape de construction est tracée dans **tmp/work** et les fichiers intermédiaires correspondant à la compilation des composants sont conservés, ce qui peut occuper plusieurs dizaines de Go sur la machine de développement. Pour réduire la taille de **tmp/work** au strict minimum (soit tout de même 1 Go) on peut ajouter la directive :

```
INHERIT += “rm_work”
```

### **Ajout du gestionnaire de paquets**

Si l’on désire disposer d’un gestionnaire de paquets binaire sur la cible, on peut ajouter la ligne suivante :

```
EXTRA_IMAGE_FEATURES += “package-management”
```

On peut également spécifier le format de paquet utilisé qui par défaut est **package_rpm**.

```
PACKAGE_CLASSES = “package_ipk”
```

### **Ajout d’espace libre sur la cible**

La taille du root-filesystem est calculée par Yocto au plus juste et nous avons vu lors de notre test qu’il restait à peine 5 Mo d’espace disponible. Si l’on veut augmenter l’espace libre on peut indiquer le volume à ajouter (en Ko).

```
IMAGE_ROOTFS_EXTRA_SPACE = “50000”
```

### **Ajout d’un format de root-filesystem**

En fonction du BSP, Yocto construit les images du root-filesystem dans **tmp/deploy/images/<nom-de-cible>**. Si l’on désire ajouter un format d’image supplémentaire, on peut utiliser la variable **IMAGE_FSTYPES**. Dans l’exemple qui suit on produit en plus une image au format Initramfs (soit une archive CPIO compressée).

```
IMAGE_FSTYPES += “cpio.gz”
```

### **Construction d’une image en lecture seule**

Dans de nombreux cas de figures, il est judicieux que le root-filesystem installé sur la cible en production soit en lecture seule. Pour cela il suffit d’ajouter la ligne suivante :

```
EXTRA_IMAGE_FEATURES += “read-only-rootfs”
```

# **Conclusion**

Dans cette première partie nous avons pu voir les rudiments de l’utilisation de Yocto après une brève comparaison avec d’autres outils comme Buildroot. Dans un prochain article nous verrons comment construire nous-même des recettes afin d’enrichir notre image.

### **Bibliographie**

*[1] Buildroot sur *[https://buildroot.org](https://buildroot.org)
 *[2] OpenEmbedded sur *[http://www.openembedded.org/wiki/Main_Page](http://www.openembedded.org/wiki/Main_Page)
 *[3] Projet Yocto sur *[https://www.yoctoproject.org/](https://www.yoctoproject.org/)
 *[4] OpenWrt sur *[https://openwrt.org/](https://openwrt.org/)
 *[5] PTXdist sur *[https://www.pengutronix.de/en/software/ptxdist.html](https://www.pengutronix.de/en/software/ptxdist.html)
 *[6] LTIB sur *[http://ltib.org/](http://ltib.org/)
 *[7] Wind River Linux sur *[https://www.windriver.com/products/linux/](https://www.windriver.com/products/linux/)
 *[8] Projet GENIVI sur *[https://www.yoctoproject.org/product/genivi-baseline](https://www.yoctoproject.org/product/genivi-baseline)
 *[9] Automotive Grade Linux sur *[https://www.automotivelinux.org](https://www.automotivelinux.org)
 *[10] Liste des layers officiels sur *[http://layers.openembedded.org/layerindex/branch/master/layers](http://layers.openembedded.org/layerindex/branch/master/layers)
 *[11] Layer du BSP Raspberry Pi sur *[http://git.yoctoproject.org/cgit/cgit.cgi/meta-raspberrypi](http://git.yoctoproject.org/cgit/cgit.cgi/meta-raspberrypi)
 *[12] Manuel de référence Yocto sur *[http://www.yoctoproject.org/docs/2.4.1/ref-manual/ref-manual.html](http://www.yoctoproject.org/docs/2.4.1/ref-manual/ref-manual.html)


