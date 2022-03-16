# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

## Question 1: Le bug de la sonde spatiale Mariner 1

La sonde devait être mise en orbite autour de la planète Mars, au lieu de s’approcher de cette planète à 226km, ce passage s’est fait à 57km.
De ce fait, le système de propulsion à surchauffé et s’est désactivé. Le système de propulsion empêche le moteur de finir de brûler et propulse la sonde en dehors de l’atmosphère martienne.

Le problème est dû à cause d’une erreur d’unités, en effet, les 2 équipes qui travaillaient sur le projet, étaient de différentes nationalités. L’un utilisait le système métrique anglais, et l’autre le système métrique américain. Les ajustements de trajectoires étaient donc faussés.
Le bug est global, car il surviendra à tous les coups pour chaque sonde utilisant ce programme.

## Question 2: Le bug de la collection 769

C’est bug global, il est dans une classe de test et concerne 3 fonctions. Ce bug est global car il peut apparaitre chez tous les utilisateurs.

Dans les fonctions de tests il y a une vérification avec un assertEquals d’une map.toString() et d’une chaine de caractères. Cependant une map peut avoir les clés dans un ordre non défini et donc l’assertion ne sera pas toujours vraie.
Une modification a été apportée pour vérifier toutes les valeurs possibles de la map.

## Question 3: Netflix
Un des buts importants de Netflix est de tester son service de disponibilité, afin qu’il soit opérationnel à n’importe quel moment pour ses utilisateurs. Pour ce faire Netflix utilise le service interne Chaos Monkey, qui va causer de manière aléatoire, des pannes d’environnement et des services de production, pendant les heures de travails des ingénieurs, afin qu’ils réagissent rapidement et peuvent se préparer en cas d'une panne réelle.
Cette méthode de travail est très efficace car les ingénieurs conçoivent leurs services de manière à gérer les défaillances d'instances de manière systématique.
Netflix possède des logiciels de déploiement qui fonctionnent dans plusieurs régions géographiques (Virginie du Nord, Oregon et Irlande), si une panne se situe dans une de ses régions, Netflix peut utiliser les logiciels de déploiement des autres régions, donc de rediriger le trafic vers une autre région.
Voici quelques expériences que font Netflix pour tester la fiabilité de leurs services : mettre fin à des instances de machines virtuelles, injecter de la latence dans les demandes entre services, faire échouer des requêtes entre services, faire échouer un service interne.
Le fait que Netflix travail avec Chaos Monkey implique que c’est les développeurs qui provoquent des bugs, et peuvent les corriger avant que ses bugs apparaissent. 
Les entreprises comme Amazon, Google, Microsoft et Facebook, appliquent des techniques similaires pour tester la résilience de leurs propres systèmes.

## Question 4: WebAssembly

Le WebAssembly, un langage de bas niveau pour le web, conçu pour compléter JavaScript avec des performances supérieures, permet d’avoir une représentation compacte, une validation et une compilation efficace, ainsi qu'une exécution sûre avec peu ou pas de frais généraux.

WebAssembly est une abstraction sur le matériel moderne, ce qui le rend indépendant du langage, du matériel et de la plate-forme, avec des cas d'utilisation qui dépassent le simple cadre du Web.

Le code binaire du WebAssembly est déjà compilé lorsqu’il arrive au navigateur, ce qui implique que son parsing est plus facile à utiliser, 20 fois plus rapide que JavaScript, il est donc plus facile à charger et à exécuter. Le code WebAssembly peut être exécuté à une vitesse quasi native sur différentes plates-formes. 

## Question 5

La spécification mécanisée est utile pour les propriétés de solidité de type énoncées dans le groupe de travail. On utilise Isabelle pour faire ça.
La spécification a révélé une déficience dans la spécification de WebAssembly qui sabotait la solidité du système de types.

Les développeurs ont créé une preuve mécanisée pour les propriétés de solidité de type énoncées dans le groupe de travail. Afin de compléter cette preuve, plusieurs lacunes dans la spécification officielle du WebAssembly, découvertes par le travail de preuve et de modélisation, ont été éliminées.

Les preuves de correction de base permettent de valider expérimentalement la spécification mécanisée en utilisant l’interpréteur exécutable, à la fois en exploitant la suite de tests de conformité officielle de WebAssembly et en menant des expériences de fuzzing.

Les tests de validation ont été effectués au niveau de l’interpréteur, pour essayer de découvrir différents bogs sémantiques dans les moteurs WebAssembly commerciaux.
Les tests ont été effectués à l’aide de l’outil CSmith, un outil de génération de cas de tests, et à l’aide de la chaîne d’outils officielle Binaryen pour convertir les tests C générés en WebAssembly.



