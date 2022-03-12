# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1.  Le bug des missiles Patriot

    Le 25 février lors de la guerre du golf, un scud irakien (un missile balistique courte portée) attaque une base américaine. L’attaque aurait dû être interceptée par le           système radar des missiles Patriot en service. Mais un bug logiciel due à un calcul non précis depuis le lancement dû à un cumul d’erreurs arithmétiques d’arrondis a empêché     le bon fonctionnement.

    Tout simplement car les dixièmes de secondes utilisé lors des calculs était converti en secondes.
    La durée de batterie étant de plus de 100h les approximations successive ont causé un grand décalage.
    En effet la représentation de 1/10 était de 0.000000095 ce décalage sur 100h donne : 0.000000095 × 100 × 60 × 60 × 10 = 0.342.
    Cela cause environ un décalage de 600 mètres du missiles irakien.

    Ce bug est global car il apparaît chez tous les missiles patriotes. Il a causé 28 morts et des centaines de blesser.

    Le bug avait été repéré avant cette incident mais la solution de contournement avait été de relancer le système régulièrement. La mise à jour pour résoudre ce bug est arrivé     quelque jours après l’incident.

    Pour moi, des tests sur une longue durée avec une simulation de l’environnement(missile ennemis) auraient pu facilement trouver le problème.

    Source : https://horustest.io/blog/les-10-bugs-informatiques-les-plus-couteux-de-l-histoire/

    https://aviationsmilitaires.net/v3/forum/questions-g%C3%A9n%C3%A9rales-et-techniques-41/topic/echec-dinterception-des-missiles-patriot-en-1991-1536/?post=63884#post-63884

2.  CollectionUtilsTest.getFromMap() is flaky : collections-775

    Le bug provient directement d’une classe de test. 
    Un assertEquals qui n’a pas le résultat voulu.

    Le assertEquals vérifie les valeurs qui sont produites par un iterator. Cependant à chaque appelle de la fonction qui récupère la valeur un nouvelle iterator est créé et si     il y a 2 appel consécutifs alors la valeur retourné n’est pas complète.

    La résolution a été de créé un nouveau test pour les Map ordonné.

    Le bug était global et un nouveau test à donc été créé.

3.  Netflix utilise le Chaos Engineering pour tester son service de disponibilité pour ses utilisateurs. C’est-à-dire qu’il met volontairement son service hors-service afin de trouver des solutions directement, si un jour ce problème apparait réellement.

    Netflix utilise l’outil Chaos Monkey pour rendre ses services indisponibles. L’outil va en effet causer de manière aléatoire, des pannes de service et d’environnements           pendant les heures de travails des ingénieurs, afin qu’ils se préparent si un jour le service tombe en panne réellement.
    Netflix possède dans différentes régions du monde, des logiciels de déploiement, ce qui implique que si un service est en panne, ils peuvent utiliser un autre service pour       rediriger le trafic.

    Pour tester la fiabilité de leurs services, Netflix met fin à des instances de machines virtuelles, injecte de la latence entre les services et font échouer des requêtes         entre services.

    Netflix n’est pas la seule entreprise à utiliser le Chaos Engineering, Amazon, Google, Microsoft et Facebook utilisent le même genre de technique.

4.  WebAssembly est un langage bas niveau, conçu pour compléter JavaScript, tout en ayant de meilleures performances en matière de rapidité et de compilation. WebAssembly est indépendant du langage, du matériel et de la plate-forme, avec des cas d'utilisation qui dépassent le simple cadre du Web.

    Il utilise un code binaire qui est déjà compilé, ce qui implique qu’il est 20 fois plus rapide que JavaScript, il est donc plus facile à charger et exécuter

5.  La spécification mécanisée est utilisée pour les propriétés de solidité de type énoncées. Elle a révélé une déficience dans la spécification de WebAssembly qui sabotait la solidité du système de types.

    Une preuve mécanisée a été conçue, permettant de découvrir des lacunes dans la spécification officielle du WebAssembly.
    Les preuves de correction permettent de valider la spécification mécanisée en utilisant l’interpréteur exécutable et en exploitant la suite de tests de conformité officielle     de WebAssembly, également en menant des expériences de fuzzing.

    Les tests de validation sont conçus pour découvrir des bugs sémantiques dans les moteurs WebAssembly, ils sont faits au niveau de l’interpréteur. Ils sont réalisés à l’aide     de l’outil CSmith, qui permet de générer des cas de tests, et également à l’aide de la chaîne d’outils officielle Binaryen pour convertir les tests C générés en WebAssembly.

    Les tests ont été effectués à l’aide de l’outil CSmith, un outil de génération de cas de tests, et à l’aide de la chaîne d’outils officielle Binaryen pour convertir les         tests C générés en WebAssembly.
