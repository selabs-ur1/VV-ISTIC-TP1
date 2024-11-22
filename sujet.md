# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Lors du premier vol de Ariane 5 en 1996, la fusée explose après 37 secondes de décollage.
Ce bug est du à la conversion d'une valeur en virfule flottante 64 bits en un entier signé 16bits ce qui à conduit à un integer overflow non géré, e qui à entrainé l'arret de l'ordinateur principal et de l'ordinateur de secours tout deux équipés du même code. plus précisement l'ordinateur à interpreter les donées corrompues comme des instructions réelles, ce qui à entrainer une déviation des moteurs.

Local ou global :
Le bug est local, il ne se produit que dans le composant de navigation inertielle mais ses conséquences ont été globales car il à entraîner la destruction de la fusée et de ses satellites. La manifestation de ce bug à eu 3 conséquences : 
L'arrêt immédiat du système principal et de secours.
L'envoi de données incorrectes au système de contrôle de vol.
Des commandes de déviation moteur extrêmes, causant des contraintes structurelles au-delà des capacités de la fusée​

Répercussions : l'ESA et ArianeGroup ont perdus environ 500 millions d'euros et évidement une perte de crédibilité dans le milieux aerospatial.
Pour les utilisateurs finaux (les scientifiques) : une prise de retard de plusieurs années du à la perte de satellites

Test possible : Oui. Ce bug aurait pu être détecté si des tests avaient été effectués avec des scénarios de charge simulant les nouvelles conditions de vol d'Ariane 5. Le logiciel, réutilisé d'Ariane 4, n'avait pas été testé pour les vitesses plus élevées atteintes par Ariane 5. De plus, la fonction en question, inutile après le décollage, aurait pu être désactivée pour éviter toute erreur inutile​

2. Apache commons Collections (Collections-769) est une carte jira pour la résolution d'un bug sur une classe de test. La classe "unmodifiableMultiVluedMapTest" avaient des tests qui échouaient de manière intermittente parcequ'il verifiaient une méthode "map.toString()" et plus précisement son return pour verifier qu'il s'agissait d'une chaine de caractère spécifique mais cette chaîne dépendait de l'ordre des élements dans la carte. Ce qui faisait que plusieurs outputs valides pouvaient etre générées mais que le test ne les reconnaissait pas comme correct, d'ou le terme "flaky" pour instable

Local ou Global : C'est un bug local, il concerne la spécifiquement la logique de test dans la partie des tests unitaires. Ca n'affecte pas directement le fonctionnement de l'application pour les utilisateurs finaux.

Résolution : Le correctif présent à été de modifier l'assertions dans les tests pour qu'elles soient insensibles à l'ordre des élements, on ne verifie donc plus directement la chaine généré par map.toString() mais on verifie qu'il y a bien des valeurs attendus indépendamment de leur ordre.

test supplémentaires : Le contributeur à la résolution du bug à mis à jour les tests comme expliqués au dessus dans la résolution, mais je n'ai pas trouver de trace de nouveaux tests.

3. Netflix, dans sa logique de chaos engineering à utiliser l'outil "Chaos Monkey" pour réaliser des experiences et simulé divers types de pannes dans leurs systemes, notamment l'arret de serveurs individuels, la panne de réseau et la défaillance des bases de données. Ces experiences ont nécéssitées un environnement contrôlé en production, notamment avec un monitoring de l'infrastructure, des mécanismes de protection qui servaient à eviter des pannes catastrophiques non maitrisées et une copie de la base de donées qui réplique des conditions réelles sans affecter directement l'utilisateur. Ces experiences ont permis d'observer des variables comme le taux de réponses de certains services, le débit réseau, le comportement des utilisateurs ou la disponibilité de certainnes fonctionnalitées comme la lecture vidéo et les recommandations.

Le résultat à permis à netflix de mettre en lumière certainnes choses, L'identification de faiblesses non détectées lors des tests classiques, la conaissances de problèmes pour pouvoir réduire le temps de rétablissement lors d'incident réel.

Netflix n'est pas le seul à utiliser ce genre de techniques, Amazon, Google et Microsoft utilise également ce système

4. Wasm et sa spécification formelle ont plusieurs avantages, en effet, la formalisation permet de minimiser les erreurs d'interprétation grâce à une description précise et non ambigue de son comportement, ainsi de nouvelles implémentations sont garanties de suivre les règles en place. Wasm valide par exemple les types et les contrôles de flux pour prévenir des comportements non définis ou dangereux. Le fait que Wasm est indépendant de l'environnement d'exécution, la création de programmes est compatibles avec tout les navigateurs, indépendamment des spécificités d'une plateforme.
Malgré la formalisation, les tests restent essentiels, une spec formelle définit ce que l'implémentation doit faire mais les tests vérifient alors que le code respecte réellement ces exigences. les tests fonctionnels et d'intégrations restent donc très important.

5. L'article de Conrad Watt nous donnes la liste des avantages suivant dans l'article "Mechanising and veryfying the webAssembly specification" :
Cette spécification à été formalisée à l'aide de l'outil isabelle, qui a permis de prouver la solidité et le respect des règles de typage. l'auteur parler aussi de techniques comme le fuzzing pour valider les interprétations de webAssembly

La mécanisation permet de prouver formellement des propriétés telle que la preservation et le progress, cela permet de valider des programmes après plusieurs étapes d'éxécution. Ce processus à permis de mettre en evidences des failles dans la spécification de base, notamment aux instructions administratives, ces corrections ont été intégrées par la suite dans la spécification ce qui à permis de rendre wasp plus robuste et de résoudre ce genre de faille.

Les artefacts dérivés de webAssembly sont un interpréteur avec les preuves formelles de sa correction. et un vérificateur des types qui garantit la cohérence des programmes validés.

Conrad Watt nous explique que bbbien que la mécanisation améliore la fiabilité de WebAssembly, elle ne remplace pas les tests, les spécifications formelles sont un socle solide mais des implémentations peuvent encore introduire des erreurs, c'est ici que les tests prennent toutes leur utilité pour verifier la conformité de ces implémentations.
