---
layout: post
url: https://medium.com/@/ad1c122ac201
title: "Wattmètre connecté pour le Smile Trip Tour 2018"
subtitle: Conception Hardware et témoignage
slug: wattmètre-connecté-pour-le-smile-trip-tour-2018
description: "Les objets connectés présentent de plus en plus d'intérêt pour des utilisateurs très variés. Principalement utilisés à des fins de monitoring, ils permettent de remonter des données dans le Cloud à…"
tags:
- sun-trip-tour
- embedded-systems
- electric-bike
author: mathi
---

### Les objets connectés présentent de plus en plus d’intérêt pour des utilisateurs très variés. Principalement utilisés à des fins de monitoring, ils permettent de remonter des données dans le Cloud à moindre coût pour analyser des tendances, repérer des patterns ou détecter des anomalies de fonctionnement.

*Leur faible consommation énergétique leur permet de rester sur site jusqu’à plusieurs années sans nécessiter de maintenance, ce qui appuie leur utilisation de plus en plus forte, considérée comme innovante.*

*Dans cet article, nous allons nous intéresser à un objet connecté actuellement en développement dans le cadre du « Smile Trip Tour », trouvant son application dans le domaine des Vélos à Assistance Electrique (VAE) solaire : le Wattmètre Connecté (WMC).*

# Fonctionnalités et contraintes environnementales

En faisant un rapide inventaire du matériel présent sur le vélo solaire, on se rend compte qu’il existe quatre éléments principaux permettant son bon fonctionnement : le panneau solaire, le contrôleur MPPT, la batterie et le moteur dans le pédalier.

![Eléments principaux d’un vélo solaire.](/assets/images/posts/1*MA-PcPvUx4Ab5kI23qPTyA.png)

Le panneau convertit le rayonnement solaire en énergie électrique qui est dans un premier temps envoyée sur le MPPT.

Le MPPT (Maximum Power Point Tracking, en bas à gauche sur le schéma) est un type de contrôleur permettant d’optimiser la puissance délivrée par le panneau solaire, en adaptant la tension de chaque cellule photovoltaïque individuellement. C’est l’intermédiaire entre le panneau solaire et la batterie. Il fournit notamment une tension aussi constante et lisse que possible à la batterie pour éviter de l’endommager et la recharger au mieux.

La batterie fournit ensuite du courant électrique au moteur qui peut alors produire un couple pour aider le cycliste à avancer en montée. Ce courant est très élevé au démarrage du moteur pour vaincre l’inertie, puis devient constant et d’une valeur plus raisonnable quand le cycliste roule à une allure fixe avec l’assistance électrique activée.

Le moteur a également besoin d’une tension d’alimentation constante en provenance de la batterie (en général 36 V ou 48 V selon les modèles).

La principale fonction du wattmètre connecté est d’effectuer des mesures de courant et tension afin de produire diverses statistiques — notamment de puissances instantanées consommées par le moteur et produites par le panneau solaire — en deux points de prélèvement différents. Sur le vélo, il faut le brancher en série entre le MPPT du panneau solaire et la batterie d’une part, et entre la batterie et le moteur d’autre part, pour mesurer les courants entrant et sortant. Il faut également pouvoir désactiver la mesure côté panneau solaire pour rendre le wattmètre adaptable aux VAE classiques.

![Positionnement du WMC (cadre vert) dans la structure du vélo solaire.](/assets/images/posts/1*bke5-F-bLxby3nhfKqL2kg.png)

Une fois que l’acquisition des données (toutes les 0,25 s) s’est bien passée, il faut pouvoir les stocker dans la mémoire du WMC pour les transmettre toutes les 0,5 s en Bluetooth Low Energy (BLE) à la *Gateway* (la carte électronique gérant le tableau de bord du coureur pour afficher les données et les envoyer dans le Cloud pour analyse). Afin de garantir la cohérence et l’exploitation des données, le wattmètre connecté devra leur associer une date et une heure, qui correspondra au moment où la mesure a été prise. C’est l’horloge temps réel du microcontrôleur (real time clock, ou RTC en anglais) qui se chargera de la gestion du temps pour l’appareil.

Enfin, le dispositif devra être alimenté par la batterie du vélo solaire et consommer un minimum d’énergie en fonctionnement normal. Le budget énergétique du WMC est donc restreint à 0.5W continus et 1 W crête lorsque tous les composants sont utilisés en même temps.

![Le WMC au coeur de l’architecture du projet.](/assets/images/posts/1*BexfSkcLtSBnXKrYSeBIbA.png)

Au niveau des contraintes techniques, une étude du marché des batteries de VAE a montré que les tensions mises en jeu varient entre 36 V et 48 V continus en fonctionnement normal, tandis que les courants délivrés par les batteries les plus puissantes ne dépassent pas 20 A en continu (et environ 30 A crête). Des batteries plus puissantes existent, mais ne concernent pas les VAE — en fait, la législation impose aux VAE d’avoir un moteur d’au maximum 250W et pouvant offrir une vitesse de 25 km/h. Au-delà de cette vitesse, le moteur d’assistance doit cesser de fonctionner. Les véhicules deux-roues équipés de batterie plus puissante ne sont pas considérés comme des vélos, mais comme des cyclomoteurs et ne font pas l’objet du Smile Trip Tour.

Sur la route du Smile Trip Tour, le wattmètre connecté sera soumis à des vibrations et à des chocs, ainsi qu’aux intempéries. Le boîtier et la connectique devront donc être résistants et étanches, mais les composants eux-mêmes devront pouvoir opérer entre -20 °C et +70 °C.

# L’architecture matérielle

La figure ci-dessous présente le schéma de principe pour l’architecture fonctionnelle et matérielle du WMC :

![Schéma électrique simplifié du WMC.](/assets/images/posts/1*GEPFvK3ZxbF9z6oiVCOqTA.png)

![Schéma de principe d’un tore de mesure.](/assets/images/posts/1*UFLgVfz-Ju47kNA1FsPAbg.png)

Le choix de la technologie des capteurs de courants (symboles « A » représentés en rouge sur le schéma) s’est porté sur des [capteurs à Effet Hall ](https://fr.wikipedia.org/wiki/Capteur_%C3%A0_effet_Hall)soudables sur PCB (Printed Circuit Board, l’acronyme anglais courant pour dire « circuit imprimé ») ; cette solution permet d’éviter de fragiliser la connectique ou de compromettre l’étanchéité du boîtier, contrairement à l’utilisation d’une [bobine de Rogowski](https://fr.wikipedia.org/wiki/Enroulement_de_Rogowski) ou d’un tore de mesure. Un [ACS722KMA-40AB](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwiL1r3Zk6jcAhWG2qQKHTANCgoQFggsMAA&url=https%3A%2F%2Fwww.allegromicro.com%2F~%2Fmedia%2FFiles%2FDatasheets%2FACS722KMA-Datasheet.ashx%3Fla%3Den%26hash%3D5474B5C571D7FEE83E60A608FED45A5FCC5BCDC6&usg=AOvVaw3RUOj4sTAXkTmWMCdR44dF) d’Allegro (soutenant 40 A maximum) fera l’affaire.

Comme les différentes notices de batteries de VAE ne sont pas toujours claires sur la présence d’un potentiel commun (bornes « — » côté recharge et côté consommation reliées à l’intérieur de la batterie), deux ADCs externes (Analog to Digital Converters, ou convertisseurs analogique-numérique) seront utilisés en mode différentiel pour mesurer la tension aux bornes de la batterie côté MPPT et côté moteur. Un pont diviseur de tension de part et d’autre de la chaîne de mesure adapte la tension vue en entrée et en sortie de la batterie (de l’ordre de 50 V maximum) à la plage d’entrée des ADCs de manière proportionnelle. A titre d’exemple, dans le cas d’un ADS1015-Q1 de chez Texas Instruments, la plage d’entrée peut être réglée de ± 0,256 V à ± 6 144 V, et de manière générale, il est rare de voir des ADCs avec une plage de tension d’entrée supérieure à 15 V. Les résistances du pont diviseur sont dimensionnées pour dissiper moins d’un dixième du budget de puissance de l’appareil. Le but est que les ADCs voient 0 V s’il n’y a pas de tension et, par exemple, 2 096 V si la tension est maximale (environ 60 V).

Le module Bluetooth RN4871-V/RM118 permet ensuite la transmission des données à la Gateway, qui devra les afficher et les remonter dans le Cloud en LoRa (pendant la course) ou Wi-Fi (après la course).

Le wattmètre est alimenté par la batterie du vélo (« Power supply » figurant en jaune sur les schémas). Lorsque la batterie est débranchée (à la fin de la course ou à cause d’un choc pendant que le vélo roule), une pile de secours permet de conserver l’horloge temps réel et quelques registres de contexte sous tension (1,8 V suffisent sur un microcontrôleur STM32). Au niveau du développement, il est intéressant d’avoir accès à un port USB capable d’alimenter la carte afin de tester le software plus facilement (communication sur le bus [I²C](https://fr.wikipedia.org/wiki/I2C) entre le microcontrôleur et les convertisseurs analogique-numérique pour récupérer les mesures des capteurs de courant, configuration du module Bluetooth). Ce port ne sera pas accessible par l’utilisateur final.

![Schéma de principe montrant la gestion de l’alimentation vue par l’utilisateur.](/assets/images/posts/1*0w22I55-ZVXcWLhRV5nuIQ.png)

Enfin, des précautions sont prises pour protéger le boîtier des surtensions transitoires typiques des ESD (décharges électrostatiques), qui peuvent arriver en branchant les câbles. Le but est de coller au maximum au niveau 2 de la norme [IEC 61000–4–2](https://en.wikipedia.org/wiki/IEC_61000-4-2) relative aux ESD : même si le wattmètre ne résistera pas à la foudre, il doit pouvoir être manipulé et branché/débranché sans précautions particulières de l’utilisateur. Les composants de protection utilisés sont des TVS (transient voltage suppressors), aussi appelées « diodes transil » : sous tension nominale, elles sont en circuit ouvert et n’affectent pas le fonctionnement du montage. En revanche, en cas de pic de tension indésirable, elles deviennent passantes et redirigent le courant directement à la masse, tout en présentant une tension beaucoup plus faible (phénomène d’écrêtage des surtensions transitoires). Les composants du wattmètre sont ainsi protégés !

# Aspect logiciel

Je n’ai pas encore évoqué le choix du microcontrôleur : le WMC utilisera le STM32F105 de la « Connectivity Line » de ST Microelectronics. Ce MCU (Micro-Controller Unit) supporte différents canaux de communication (I²C, UART, SPI) et permet de faire tourner un système d’exploitation en temps réel, ou « RTOS. » Celui qui sera utilisé est Open Source — il s’agit de [*FreeRTOS*](https://www.freertos.org/), qui est écrit en C et se prête bien aux applications embarquées.

Ce dernier point vient étayer le partenariat privilégié que Smile entretient avec AWS (Amazon Web Services). Amazon a récemment investi dans le projet et développé un environnement « Amazon FreeRTOS » facilitant l’accès à l’Edge Computing (traitement de données en réseau local) et au Cloud Computing (traitement de données sur des serveurs distants type Clouds, connectés à Internet), basé sur l’infrastructure Amazon. Le WMC utilisera simplement la version originale purement Open Source de FreeRTOS, mais on peut toujours imaginer une évolution où la Gateway serait remplacée par les fonctionnalités Amazon.

L’architecture logicielle sera basée sur un kernel préemptif gérant des tâches périodiques ayant un niveau de priorité et un accès aux ressources matérielles bien définies. Parmi elles, on trouve automatiquement :

![](/assets/images/posts/1*7fFXnqMdiIkjVpqnsSSuHA.png)

# Réalisation du WMC — “Behind the scenes”

![Marie THIBAUT, stagiaire chez Smile](/assets/images/posts/1*yn5wmXbh-ru02b5yhWoMkg.jpg)

Nouvelle arrivée chez Smile, je travaille au sein de l’équipe « Embedded and Connected Systems » à Grenoble dans le cadre de mon stage de fin d’études. Après deux ans de classes préparatoires, j’ai intégré l’Ecole Nationale Supérieure de l’Électronique et de ses Applications (ENSEA, à Cergy-Pontoise) pour étudier l’électronique et me spécialiser en mécatronique et systèmes embarqués. Je m’intéresse particulièrement à la programmation bas-niveau et à l’électronique numérique, mais sur mon temps libre je fais surtout beaucoup de sport… D’où mon intérêt pour le Smile Trip Tour !

La conception hardware et la réalisation physique du WMC constituent la première étape de mon stage. Actuellement, le schéma électrique de la carte est en phase de validation (quelques retouches sont sûrement encore à faire) et je commence le design d’empreintes pour pouvoir passer au routage des composants (le fait de dessiner les pistes entre chaque composant sur la carte).

**Quelles difficultés ai-je rencontrées pendant la conception Hardware ?**

C’est principalement « des trucs qu’on n’apprend pas à l’école », en fait. Jusqu’à maintenant, le plus difficile pour moi concerne la nomenclature des packages standards (typiquement SOIC16, SMD 1206, LQFD64…), le choix des connecteurs, et la logistique.

![Exemple de package standard : un SOIC16](/assets/images/posts/0*TIKm4dO-hRfNPM3A.jpg)

Retenir à quoi ressemble chaque package pour se rendre compte si le composant sera facile à souder ou prendra trop de place sur le PCB prend du temps, c’est un réflexe qui ne se développe qu’en ayant parcouru plusieurs dizaines de datasheets et ça peut paraître rébarbatif. Mais on s’y fait — le PCB en dépend.

Au niveau des connecteurs, c’est aussi un peu casse-tête au début. USB-A, B, C, AB ? Mini, micro ou USB classique ? Comment monter et brancher des connecteurs Anderson sur la carte ? Ce genre de questions nécessite de passer du temps à lire des articles et regarder des exemples de schémas pour mieux comprendre.

![Différents types de connecteurs USB.](/assets/images/posts/1*9rsmVwrXc2lG0kUhDiXG8g.png)

Et enfin… Tout le matériel du STT ne se trouve pas forcément à l’agence de Grenoble ! Un vélo (et sa batterie, son moteur, son panneau solaire…) se trouve à l’agence de Lyon, un autre est démonté dans l’atelier de Grenoble sans batterie ni moteur… Il faut donc avoir assez d’imagination pour se représenter le wattmètre dans son contexte, et ne pas hésiter à communiquer régulièrement avec les ingénieurs des autres équipes.

**Construire le WMC, est-ce un challenge technique motivant ?**

Clairement ! Ce projet en est vraiment pluridisciplinaire : il mêle Hardware, Software, gestion de projet, communication, électronique numérique et analogique, microélectronique, électrochimie… Il s’inscrit aussi dans une volonté de Smile de former de futurs ingénieurs sur des technologies émergentes de pointe. (En plus ça me donne l’occasion de travailler avec un OS temps réel léger sur un SMT32, que demander de plus ?)

La dimension Open Source du projet est également gratifiante. L’outil que j’utilise pour créer le circuit électrique et le PCB, [KiCad](http://kicad-pcb.org/), est un logiciel open source similaire à Altium Designer ou Eagle. Je ne l’avais encore jamais utilisé jusqu’alors (j’étais sous Alitum jusqu’à ce que ma licence étudiante expire), et maintenant j’ai bien mes repères dessus. C’est toujours un plus d’apprendre à utiliser de nouveaux logiciels Open Source comparés à des versions propriétaires, ne serait-ce que pour pouvoir faire de petits projets rapides chez soi après. La possibilité de contribuer à des projets Open Source une fois le travail bien avancé offre de la visibilité et une certaine reconnaissance dans le monde de l’embarqué. Cela permet aussi un dialogue avec d’autres contributeurs pour améliorer sans cesse les développements effectués.

![Logo du logiciel de conception assistée par ordinateur open source “KiCad”](/assets/images/posts/0*E4U8Ppt6UosWdCZV.jpg)

En R&D, qu’il s’agisse de hardware ou de software, il faut souvent trouver des compromis et des workarounds pour livrer une fonction particulière en respectant un cahier des charges. Il faut pouvoir s’adapter rapidement à des changements de cahier des charges ou de priorité sur telle ou telle fonctionnalité du produit suite à des réunions d’équipe, cela donne un côté dynamique et jamais ennuyeux au travail quotidien. C’est une dimension de la gestion de projet qu’on retrouve peu en école, car le cahier des charges y est le plus souvent statique et les solutions techniques envisageables sont déjà connues du corps enseignant. Ce challenge de conception donne tout son sens à la formation d’ingénieure que j’ai reçue, et permet de confirmer mon intérêt pour le développement électronique.

Have you enjoyed it ? If so don’t hesitate to 👏 our article or s[ubscribe to our Innovation watch newsletter ](https://www.getrevue.co/profile/smileinnovation)!
You can follow Smile on F[acebook,](https://www.facebook.com/smileopensource) T[witter ](https://www.twitter.com/GroupeSmile)& Y[outube.](http://www.youtube.com/user/SmileOpenSource)


