# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

# Partie 1 : Surchauffe des processeurs Intel

Après la sortie de processeurs de 13e et 14e génération, Intel est rapidement entré dans une polémique concernant des problèmes en jeu. L’affaire s'est conclue le 22 Juillet dernier et à durée un près de deux ans. 

En 2022 des problèmes d'instabilités sont reportés par les utilisateurs sur des forums tels que Overlock.net ou Avandtech. Ces alertes font état de plantages ou remontent des erreurs “Out of Video memory” lors de l’utilisation d’une machine composée de processeur i9-13900 K. Ce n’est qu’en 2023 que la mise en cause du processeur est officialisée suite à l’apparition de blue screen portant le message “Unsupported processor” ou Crash to desktop”. 

En 2024, le constructeur prend en main le problème lorsque certains développeurs d’Unreal Engine préviennent les joueurs de problèmes récurrents sur les processeurs Intel de 13e et 14e génération. L’enquête est lancée et elle s’oriente sur des problèmes de surtension provenant du microcode des processeurs. 

Intel découvre que non seulement c’est la quasi-totalité des processus de 13e et 14e génération sont touchés, mais aussi que les dommages causés sont irréversibles. 

L’erreur concernant le Blue screen a pu être corrigée avec une simple mise à jour du BIOS appliquée par les fabricants de cartes mères. Intel recommande ensuite des configuration aux utilisateurs, de façon à ne pas abîmer leurs processeur et conseil de renvoyer, grâce à la garantie, ceux déjà endommagés. 

Le 22 Juillet dernier, grâce aux retours de processeur détérioré, il est annoncé par le fabricant que l’origine à été trouvé et elle est global : 
“un algorithme de microcode entraînant de demandes de tensions incorrectes au processeur” ce qui provoque des dégâts irréversibles du silicium présent dans les processeur, ce dernier ne pouvant plus supporter les hautes tensions ou température répètent les symptômes évoqués plus haut. 

La firme n’a pas rappelé ses composants mais à étendu sa garantie de deux ans et assoupli ces règles de retour. 

Le problème n’a été repéré qu'après avoir touché un nombre important d'utilisateurs et donc après avoir abîmé leurs machines. Un test d'utilisation intensive des processeur aurait sans doute révélé ce défaut et éviter les mise à jour BIOS demandées aux fabricants de carte mères et des retour de processeurs endommagés. Un tel test appliqué sur la 13e génération aurait aussi pu prévenir la sortie d’une deuxième génération déficiente. 

![Fandroid - processeurs Intel Instable](https://www.frandroid.com/marques/intel-materiels-accessoires/2299558_processeurs-intel-instables-tout-savoir-sur-laffaire-en-4-questions)

![https://en.wikipedia.org/wiki/2009%E2%80%932011_Toyota_vehicle_recalls#Accelerator_pedal_recall](https://en.wikipedia.org/wiki/2009%E2%80%932011_Toyota_vehicle_recalls#Accelerator_pedal_recall)

# Partie 2 : COLLECTIONS-701 StackOverflowError in SetUniqueList.add() when it receives itself

Lors de l'appel de SetUniqueList.add() il y a une récursion infini parce qu'l'objet s'ajoutait lui même. Pour corriger cela ils ont changé l'ordre d'appel de certaines instruction, de plus ils ont ignorer un test ainsi que la création d'un nouveau test.


# Partie 3 : Netflix
Netflix réalise plusieurs expériences comme : 
Un arrêt aléatoire de machines virtuelles en production (Chaos Monkey)
Simuler des défaillances entre services (FIT) 
Simuler la panne d'une région Amazon EC2 entière (Chaos Kong)
Injecter de la latence dans les requêtes entre services
Simuler des pannes dans des services internes
Ces expériences ont plusieurs prérequis : 
Définir une mesure de "l'état stable" du système
Avoir des groupes de test et de contrôle
Avoir la capacité d'observer et de mesurer le comportement du système
Avoir la possibilité d'automatiser les tests
Pouvoir exécuter leur expériences pendant les heures de travail normales pour une réponse rapide
Lors de ces expériences différentes variables sont observées, la principale est le nombre de démarrages de streaming par seconde (SPS). Il y a aussi des métriques secondaires comme la charge CPU, le temps de réponse des bases de données, le taux de création de nouveaux comptes et le comportement des services de repli (fallback).
Les principaux résultats ont été que les ingénieurs conçoivent désormais systématiquement leurs services pour résister aux pannes, ils ont mis en place des développements de mécanismes de repli efficaces, de plus ils ont acquis une meilleure compréhension du comportement du système sous stress et enfin ils possèdent une identification proactive des faiblesses du système.
Netflix ne sont pas les seules a utilisé du Chaos engineering d’autres entreprises pratiquant ces expériences, notamment Amazon, Google, Microsoft et Facebook pratiquent également des techniques similaires.

Il est possible d’adapter ces expériences à d'autres organisations par exemple :
Pour un site e-commerce (Type Amazon):
Simulation de pannes de service de paiement
Tests de surcharge du panier d'achat
Variables à observer : nombre de transactions complétées par seconde
Pour un service publicitaire :
Tests de défaillance du système de ciblage
Simulation de latence dans la diffusion des annonces
Variables à observer : nombre d'affichages publicitaires par seconde
Pour une institution financière :
Tests de repli des systèmes de trading
Simulation de pannes des systèmes de validation
Variables à observer : taux de transactions réussies

# Partie 4 : WebAssembly

Il y a plusieurs avantages à avoir une spécification formelle pour WebAssembly (Wasm), comme le fait que cela assure une certaine sécurité et prévisibilité, une spécification formelle garantit que le comportement de WebAssembly est déterministe et sûr. 
De plus, une telle spécification permet une simplicité de validation et de compilation du code, la formalisation permet aux moteurs d'exécution de valider et compiler le bytecode WebAssembly de manière efficace et optimisée.
Une spécification formelle permet aussi une portabilité et une interopérabilité tout en respectant des comportements identiques, indépendamment des architectures matérielles sous-jacentes, cela permet une exécution correcte sur différentes plateformes.
Enfin, la définition claire d’une spécification pour une clarté pour les futures évolutions du langage.
Même si la spécification formelle fournit une base solide pour garantir la sécurité, la portabilité, et la prédictibilité de WebAssembly, cela ne signifie pas que les implémentations de WebAssembly ne doivent pas être testées.
En effet, même si la spécification formelle offre un cadre et un ensemble de règles, il reste des éléments qui doivent être validés par des tests dans des situations réelles. Par exemple, les performances dans des cas réels. Bien que la spécification formelle permet en théorie une interopérabilité avec les plateformes, il faut tout de même le tester en pratique avec des contraintes matérielles. Et enfin il faut voir en pratique la gestion des erreurs et la tolérance aux pannes.
En résumé, bien que la spécification formelle du WebAssembly assure une base solide pour sa sécurité, sa portabilité et sa performance, les tests restent essentiels pour valider les implémentations concrètes et garantir leur bon fonctionnement dans des environnements variés.

# Partie 5 : WebAssembly Test

Le principal avantage d'avoir une spécification formelle pour WebAssembly (Wasm), comme le mentionne l'article, réside dans plusieurs aspects cruciaux :

  Sécurité et prévisibilité : Une spécification formelle garantit que le comportement de WebAssembly est déterministe et sûr

  Simplicité de validation et de compilation : La formalisation permet aux moteurs d'exécution de valider et compiler le bytecode WebAssembly de manière efficace et optimisée.

  Portabilité et interopérabilité : Grâce à la spécification formelle, WebAssembly peut être porté sur différentes plateformes (navigateurs, serveurs, IoT) tout en respectant des comportements identiques, indépendamment des architectures matérielles sous-jacentes. Cela permet d'assurer que le même code WebAssembly fonctionne de manière cohérente sur différentes plateformes.

  Clarté pour l’évolution du langage : Avoir une spécification formelle permet de mieux comprendre les principes sous-jacents du langage et d'orienter son évolution de manière claire et structurée. Cela réduit les risques d’incohérence ou de rupture lors des évolutions futures.

À propos des tests d'implémentations :

Même si la spécification formelle fournit une base solide pour garantir la sécurité, la portabilité, et la prédictibilité de WebAssembly, cela ne signifie pas que les implémentations de WebAssembly doivent être exemptes de tests.

En effet, bien que la spécification formelle offre un cadre théorique et un ensemble de règles strictes, il reste des éléments pratiques qui doivent être validés par des tests dans des situations réelles. Par exemple :

  Performance : Les optimisations de performance, comme celles basées sur des compilateurs Just-In-Time (JIT), dépendent de la mise en œuvre spécifique dans chaque moteur de navigateur et doivent être testées sur des cas d'utilisation réels.
  Interopérabilité avec les plateformes : L'intégration de WebAssembly dans différents environnements (JavaScript, autres langages, systèmes embarqués) doit être testée pour s'assurer que les interfaces fonctionnent correctement et que les modules WebAssembly interagissent comme prévu avec les systèmes externes.
  Résilience et gestion des erreurs : Bien que la spécification formelle vise à éliminer les erreurs évidentes, des tests pratiques sont nécessaires pour détecter des bogues ou des comportements imprévus, notamment dans des cas d’utilisation extrêmes ou non anticipés par la spécification.

![https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf](https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf)

