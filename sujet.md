# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

**REPONSE** : article datant du 14 Mai 2024

Lien de l'article : 
[Microsoft fixes Windows zero-day exploited in QakBot malware attacks](https://www.bleepingcomputer.com/news/microsoft/microsoft-fixes-windows-zero-day-exploited-in-qakbot-malware-attacks/)

Lien du bug : [Windows DWM Core Library Elevation of Privilege Vulnerability](https://msrc.microsoft.com/update-guide/en-US/advisory/CVE-2024-30051)

Il s'agit d'un bug trouvé sur Windows  et particulèrement sur le composant Desktop Window Manager (DWM) qui gère le rendu graphique de fenetre et d'animations. Il s'agit d'un bug de sécurité.
Le bug a été découvert par les chercheurs de Kaspersky. 

Explication du bug : 
    Le bug consiste en une "heap-based buffer overflow", c'est à dire que l'on dépassent la limite d'un tampon mémoire (le tas ici) qui peut mener par la suite à une execution de code malveillant en manipulant des pointeurs ou données du programme.
    Dans le cas de ce bug l'attaquant peut ensuite obtenir les privilièges systemes.

Detection du bug : 
    La detection de ce bug à été fortuite (Kaspersky travaillaient initialement sur un autre bug et ont découvert celui-ci). Il a été découvert en mettant un lien un autre bug et une référence sur VirusTotal
    Il n'y a pas clairement de "failure" identifiable.

Le bug est local car il s'appuie sur les privilièges sytèmes de la machine ciblée et se produit par injection de code sur la machine. 

Les repercussions : 
    - pour le client : Les utilisateurs étaient exposés à des attaques de logiciels malveillants (installation de logiciels malveillant supplémentaires...). Cela implique le vol de données sensibles ou la mise en place de rançon, la suppression de données... 
    - pour microsoft : Un tel bug affecte la confiance des utilisateurs dans la sécurité de Windows. Peut mener à une baisse de l'utilisation et donc une baisse du profit pour l'entreprise.





2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?


**REPONSE** :
Bug : [A ∩ B != B ∩ A when duplicates are present in a list](https://issues.apache.org/jira/browse/COLLECTIONS-359?jql=project%20%3D%20COLLECTIONS%20AND%20statusCategory%20%3D%20Done%20AND%20Priority%20%3D%20Major%20ORDER%20BY%20updated%20DESC)

Il s'agit d'un bug global car il affecte n'importe qu'elle application qui utilise la bibliotheque Collections en Java

Explication du bug :
    Le bug est une erreur sur le résultat d'une intersection. Ici dans le cas ou les listes contiennent des élément dupliqués le résultat de AnB n'est pas le même que BnA. De manière plus général, un des deux résulats de AnB est erroné d'après l'exemple fourni dans le ticket. 
    [a, b] ∩ [a, a, b, b] = [a, a, b, b]

Résolution du bug : 
    Dans la cas de l'intersection de liste1 et liste2.
    Pour résoudre ce problème, il faut en parcourant la liste 2 enlever au fur et à mesure les éléments insérer dans le résultat de la liste 1 pour ne pas avoir à les compter plusieurs fois s'il sont présent plusieurs fois dans la liste2. Pour cela, on intègre une copie de la liste1 pour ne pas modifier l'originale.

Un test a bien été ajouté pour vérifié que AnB==BnA et éviter une future regression.





3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

**REPONSE** :
Experimentations effectuées : 
    - Chaos Monkeys : choix aléatoire d'une instance de machine virtuel qui détient un service de production et arret de celle-ci pour vérifier que les services peuvent fonctionner même avec un peu de défaillance.
    - Chaos Kong : simulation d'une erreur sur tout une region de Amazon EC2 pour vérifier la résilience du système.
    - Failure Injection Testing (FIT):   mise en echecs volontaires de requêtes entre les services de Netflix pour vérifier que le système se dégrade de manière contrôlée.


Les prérequis pour les experimentations : 
    - Les expériences sont menées sur des systemes en production car il n'est pas possible de reproduire un système aussi complexe dans un environnement de test
    - Les expériences sont executé automatiquement car le système évolue continuellement 
    - s'appuyer sur des variations du monde réels
    - Il faut faire une hypothèse sur un comportement stable (steady-state behavior)
    (- Certaines expériences sont menées sur des sous-ensembles d'utilisateurs pour moduler le risque et faire des comparaisons entre les utilisateurs ayant un problème et les autres.)

Quels sont les variables observées ? 
    Netflix mesure le SPS (stream start par second)
    qui correspond à l'hypothèse steady state behavior. Il s'agit du nombre de démarrage de flux video par seconde.
    Une autre variable utilisé par Netflix est le nombre de création/connexion de compte par seconde

    D'autres métriques comme la latence des requêtes ou l'utilisation des CPU sont regardées pour voir des signes de fonctionnement dégradé pour compléter leur tests même si ce n'est pas le propre du chaos enginering.

Quels sont les résulats obtenues ?  
    - Une meilleur résilience car le singénieurs concoivent maintenant les services pour tolérer ces défaillances 
    - La degradation du service est controllée car même si certains services sont en erreurs(recommendation ...), il est toujours possible de regarder du contenu.
    - Amélioration du service en mettant en place des comportements de secours en cas de panne.

Netflix est-il le seul à faire cela ?
    Netflix n'est pas la seule entreprise à faire du Chaos Engineering. L'article mentionne qu'Amazon, Google, Microsoft, et Facebook appliquent également des techniques similaires.

Comment mettre en place les experimentations dans d'autres entreprises (variable à observer ...)  ? 
    - Il faut avoir une traffic assez important sur l'application afin de pouvoir réelement mesurer l'impact du résultat des camapgne de tests 
    - Il faut définir une variable qui mesure un comportement stable dans le temps en production par exemple pour une entreprise de e commerce on peut utiliser le nombre d'achat par seconde. Cela depend du service prodiguer.
    - En somme il parait réalisable de mettre en place du chaos engineering, il suffit de pouvoir mesurer une variable stable lié à l'utilisation de l'application.


4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 


**REPONSE** :
Quels sont les avantages principaux d'avoir une specification formel pour WebAssembly ? 

    La specification formelle permet d'avoir une base solide et sécurisée pour webAssembly. cela permet d'eviter les comportements indéfinis ou les erreurs de types par exemple. Cela garentit un comportement cohérent sur toutes les plateformes et navigateurs. Cela permet de garantir au développeur que leurs applications fonctionneront de la même manière quel que soit l'environnement d'execution.


Cela signifie-t-il que WebAssembly ne doit pas être testé ? 
    Non cela n'empêche pas de tester WebAssembly. En particulier, la specification formelle garantit la validité du modèle théorique mais pas des mises en oeuvres ! 
    Ces tests peuvent avoir pour but s'assurer de la qualité et et de la robustesse des implmentations ! En effet chaque moteur ayant ses propres optimisations, cela permet de vérifier que les optimisations ne créent pas de comportments inattendus / indésirables.



5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

**REPONSE** :
Quelle sont les avantages de la specification mécanisée ? 
    La specification mécanisée permet de verifier la correction et la precision formelle (typages ....)
    Cela permet de créer des artefacts vérifiés qui aide à vérifier le programme. Cela permet d'avoir une base solide pour le developpement future 

Cela améliore t il la specification formelle original ? 
    Oui, la spécification mécanisée a permis d'identifier plusieurs erreurs dans la spécification officielle.
    Elles ont été corrigées par les auteurs de la spécification. Par exemple, l'auteur a découvert des problèmes liés à la propagation des exceptions et à l'opération de retour (`Return`).

Quels sont les autres artefacts ?
    - un interpreteur executable 
    - un verificateur de type 

Comment l'auteur a t il vérifié la specification ? 
    Il a utilisé des preuve mécanisées (induction sur la relation de reduction ou de typage ). Il a également testé la conformité avec la specification officielle.

Cela supprime t il le besoin de tester la specification ?  
    Non, même avec une spécification formelle mécanisée, il faut réaliser des test. La spécification formelle garantit que le modèle théorique est bon, mais les implémentations peuvent toujours contenir des erreurs. Des tests sont donc nécessaires pour vérifier la conformité des implémentations à la spécification, notamment pour détecter des erreurs dans l'intégration de composants non formellement vérifiés, tels que les analyseurs et les éditeurs de liens.







