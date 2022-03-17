# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

## Question 1 : Samba et la vulnérabilité CVR-2021-44142

On a trouvé un article concernant la vulnérabilité CVR-2021-44142. Ce bug a été découvert par Orange Tsai de Devcore et concerne le logiciel Samba. Ce dernier est une implémentation du protocole SMB qui fournit l'interopérabilité des fichiers et des imprimantes pour les plates-formes Windows sur le réseau dans les ordinateurs tournant sous le système d'exploitation Unix et ses dérivés.

CVR-2021-44142 a été révélée lors du concours Pwn2Own Austin. Durant ce concours, les chercheurs sont mis au défi de trouver des vulnérabilités inconnues sur des logiciels très connus.
La faille de sécurité critique découverte permet à un attaquant d'exécuter du code à distance sur le serveur en bénéficiant des droits "root". C'est une vulnérabilité de type out-of-bounds en lecture/écriture présente dans le module VFS "vfs_fruit". Ce type de vulnérabilité signifie que le logiciel peut lire ou écrire des données après la fin, ou avant le début, du tampon prévu.

Ce type de bug s'apparente à un bug global. En effet, trois vulnérabilités distinctes ont été annoncées : une lecture hors limites du tas, une écriture hors limites du tas et un débordement de tampon basé sur le tas. Dans chaque cas, il s'agit apparemment d'un problème sur les spécifications de certaines variables. Il a fallu exploiter une combinaison de ces vulnérabilités pour obtenir un accès root. Ces vulnérabilités peuvent être déclenchées lorsque le module fruit VFS est activé dans la configuration de Samba.

Ce bug peut entraîner des conséquences non négligeables pour les utilisateurs car cela permet à une personne malveillante d'exécuter du code en tant que root sous Linux. Ainsi, l'exploitant du bug a alors toutes les permissions sur le système, bénéficiant d'accès privilégiés.

A noter que plusieurs conditions préalables non triviales doivent être remplies pour une exploitation réussie :
- la configuration d'une ressource partagée,
- l'accès en écriture activé,
- l'inclusion du module vulnérable vfs_fruit.

Or, le module vfs_fruit n'est pas chargé de base dans les distributions Linux courantes dans une configuration par défaut de Samba. Ainsi, l'exploitation de cette faille se limite aux périphériques NAS vulnérables par défaut.

Ainsi, avoir le bon cas de test aurait pu permettre de détecter cette vulnérabilité. Encore faut-il penser à ce cas de test car compte-tenu des conditions préalables nécessaires, il apparaît assez complexe de penser à un tel scénario.

Enfin, l'entreprise a apporté un patch le 31 janvier 2022 en rajoutant une fonction ad_entry_check_size(), permettant de vérifier la taille de chaque entrée lors de l'analyse du format AppleDouble. Samba a aussi ajouté l'attribut étendu Netatalk AFPINFO_EA_NETATALK à la liste des noms d'attributs privés, limitant ainsi l'attaque. Il est difficile de juger des conséquences possibles de cette faille sur l'entreprise, la découverte de cette dernière s'étant déroulé en novembre 2021. Néanmoins, il apparaît nécessaire que de nombreux fournisseurs doivent mettre à jour la version de Samba qu'ils livrent avec leurs appareils, supposant aussi pour eux de nouveaux correctifs et développements.

Sources :
https://jfrog.com/blog/cve-2021-44142-critical-samba-vulnerability-allows-remote-code-execution/
https://www.it-connect.fr/samba-une-vulnerabilite-permet-dexecuter-du-code-en-tant-que-root/
https://www.samba.org/samba/security/CVE-2021-44142.html
https://www.cvedetails.com/vulnerability-list/vendor_id-102/Samba.html

## Question 2

On a choisi le bug COLLECTIONS-734 : "Encountered an IllegalStateException while traversing with Flat3Map.entrySet()".

Il s'agit d'un bug local. Une personne a obtenu une exception de type IllegalStateException en souhaitant traverser une Map de type Flat3Map avec la méthode Flat3Map.entrySet(). Cette exception est générée à la suite d'un problème dans une méthode de la classe java Flat3Map. En effet, dans la méthode EntryIterator.remove(), deux lignes étaient interverties (le booléen indiquant une suppression était passé à true avant même d'avoir supprimé l'élément en question). Il a suffi de les inverser pour résoudre le bug.

Un test a par ailleurs été ajouté pour détecter ce bug si jamais il venait à de nouveau avoir lieu. Le développeur ayant corrigé le bug, s'est appuyé sur le code de la personne ayant détecté le bug pour créer le nouveau test.

## Question 3
Netflix est une énorme plateforme de streaming de vidéo. Leur but est de fournir un service constant de streaming vidéo, sans rupture. Reproduire les conditions d'utilisation avec tous les utilisateurs est compliqué. C'est donc pour cela qu'ils ont employé le modèle d'expérimentation Chaos Engineering qui permet de renforcer la confiance dans la capacité du système à résister à des conditions turbulentes en production.
Ainsi, Netflix gère un service interne appelé Chaos Monkey. Ce dernier sélectionne au hasard des instances de machines virtuelles hébergeant les services de production et les arrêtent.

A noter que ce service n'est actif que durant les heures de travail des ingénieurs afin de permettre à ces derniers de réagir rapidement au besoin.

#### Experience
Concrètement, les ingénieurs peuvent simuler la panne d'un cloud Amazon EC2 entier sur toute une région. Ils effectuent également des exercices de test d'injection de défaillance (FIT) où ils provoquent des demandes
entre les services Netflix voués à échouer et ils vérifient que le système se dégrade correctement. Ils peuvent aussi arrêter des services de mise en cache. Ils testent aussi comment réagit la reprise du visionnage d'une vidéo et si, en cas de problème, l'interface de Netflix propose de démarrer la vidéo au début plutôt que d'avoir accès à l'option : "reprendre à partir de l'emplacement précédent".

Ainsi, voici quelques exemples des expérimentations réalisées :
● résilier les instances des machines virtuelles
● injecter de la latence dans les requêtes entre les services
● échouer les demandes entre les services
● faire échouer un service interne
● rendre une région Amazon entière indisponible

#### Prérequis
Le "Chaos Engineering" s'articule autour de l'exécution d'expériences. Afin de concevoir une expérience il est important de préciser :
- les hypothèses
- les variables indépendantes
- les variables dépendantes
- le contexte

Ainsi 4 grands principes sont employés :
1. Élaborer une hypothèse sur le comportement à l'état d'équilibre
2. Varier les événements du monde réel
3. Exécuter des expériences en production
4. Automatiser les expériences pour qu'elles s'exécutent en continu

#### Variable

Les ingénieurs de Netflix utilisent comme métrique SPS pour "(stream) starts per second" afin de regarder combien d'utilisateurs commence à diffuser une vidéo chaque seconde. Cette métrique est le premier indicateur employé pour voir l'état de santé du système.
Une autre métrique qu'ils peuvent observer concerne le nombre d'inscription de nouveaux comptes par seconde.

#### Résultat

Un lien fort est relevé entre ces métriques et la disponibilité du système. Si le système n'est pas disponible, les utilisateurs ne peuvent pas diffuser de vidéo, ni s'inscrire au service. Ainsi, les hypothèses formulées portent sur la façon dont le traitement affectera l'état d'équilibre du système.
Ainsi, à la fin d'une expérience, soit les ingénieurs ont plus confiance en la capacité du système à maintenir le comportement attendu en présence des variables qui ont été manipulées, soit cela révèle une faiblesse et des pistes d'amélioration sont alors proposées.

#### Seul compagnie ?

Netflix n'est pas la seule compagnie à employer des techniques similaires. Ainsi, Amazon, Google, Microsoft et Facebook testent aussi la résilience de leurs propres systèmes.

#### Spéculation pour les autres entreprises

Le "Chaos engineering" est une bonne méthode afin de tester une application dans une mise à l'échelle parfois impossible. Cela est permis grâce à un travail en amont permettant de définir quelles sont les variables qui vont être modifiées et quelles sont les métriques qui doivent être observées. Ces tests sont pertinents soit pour déceler une faiblesse, soit pour s'assurer de la force du système.

## Question 4
L'efficacité et la sécurité du code sur le web sont devenus des points critiques et très importants depuis l'incroyable évolution du Web. Cependant, le seul langage intégré du Web, Javascript, ne permet pas de répondre à ces exigences. C'est pour pallier ce manque que WebAssembly a été créé. WebAssembly offre une représentation compacte, une validation et une compilation efficaces, ainsi qu'une exécution sûre, faible ou sans frais généraux.
Il est indépendant du langage, du matériel et de la plateforme, avec des cas d'utilisation qui peuvent aller bien au-delà du Web. Cette notion d'indépendance s'explique par le choix de ne pas s'engager dans un modèle de programmation spécifique. Ainsi, dès le début, WebAssembly a été conçu avec une sémantique formelle.

De nombreuses tentatives pour résoudre le problème du code de bas niveau sûr, rapide et portable sur le Web avaient été effectuées avec notamment ActiveX ou Native Client. Ces premiers essais avaient échoué car ils ne respectaient pas les propriétés qu'une cible de compilation de bas niveau est censée avoir, au contraire de WebAssembly :
- Sémantique sécuritaire, rapide et portable : sûr et rapide à exécuter, indépendant du langage, du matériel et de la plateforme, déterministe et facile à raisonner, une interopérabilité simple avec la plateforme Web
- Représentation sûre et efficace : compact et facile à décoder, fluide et parallélisable, facile à valider, à compiler et à générer pour les producteurs

En revanche, de notre point de vue, ce n'est pas parce que WebAssembly apporte autant d'avantages notamment grâce à la spécification formelle, qu'il ne faut pas tester les implémentations. Tout code doit être testé si cela est possible.

## Question 5

#### Quels sont les principaux avantages de la spécification mécanisée ?
Grâce à la spécification mécanisée, l'auteur a pu apporter une preuve de la solidité du standard de binaire WebAssembly. En effet, un des principaux objectifs était de spécifier le langage afin de le rendre accessible à l'analyse formelle et à la vérification.
Un des avantages porte sur le fait qu'il est nécessaire d'être explicite quant aux hypothèses réalisées portant sur le comportement de code non fiable s'interfaçant avec l'interpréteur vérifié. De plus, ce modèle bénéficie d'un avantage significatif dans la préservation de la proximité du "eyeball closeness". Il s'agit d'un principe de conception du modèle formel tel qu'il y ait une correspondance textuelle ligne à ligne entre la spécification officielle et la mécanisation. Ainsi, toutes les règles de réduction et de typage de WebAssembly dans la spécification incluent déjà une définition formelle.

#### Cela a-t-il aidé à améliorer la spécification formelle originale du langage ?
Cela a exposé plusieurs problèmes avec la spécification officielle de WebAssembly, influençant son développement. Ainsi, par exemple, le mécanisme par lequel une implémentation WebAssembly s'interface avec son environnement hôte n'a pas été formellement spécifiée.

#### Quels autres artefacts ont été dérivés de cette spécification mécanisée ?
Nous retrouvons d'autres artefacts définis comme un interpréteur exécutable vérifié mais aussi un vérificateur de type exécutable.

#### Comment l'auteur a-t-il vérifié la spécification ?
L'auteur a vérifié les nouvelles fonctionnalités au fur et à mesure que celles-ci ont été ajoutées à la spécification. Cela passe par la modélisation de la fonctionnalité par extension de la mécanisation. Ainsi, l'auteur et son équipe ont réalisé une mécanisation complète du noyau sémantique d'exécution et du système de type du langage WebAssembly.

#### Cette nouvelle spécification supprime-t-elle le besoin de tests ?
Cette spécification n'enlève en rien la nécessité de tester tout système ou langage. Ainsi nous constatons à la lecture de ce papier que des tests ont été portés sur l'interpréteur exécutable, porté sous le terme de validation initiale. Il apparaît donc important de porter plus de tests, par exemple pour tester les artefacts. 