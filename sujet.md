# Practical Session #1: Introduction

## Discovery of a software bug

Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

Le 19 juillet 2024, un bug provoqué par l’antivirus cloudStrike a provoqué la panne de nombreux terminaux dans le monde en provoquant des redémarrages infinis sur des machines windows server ce qui fait que ce bug est global. 

Le problème fut l’ajout d’une entrée en plus dans le channel files 291 qui ensuite était fourni dans un content interpreter, celui-ci est passé de 20 à 21 or ce content interpreter ne pouvait gérer que 20 entrées ce qui a provoqué un dépassement mémoire et donc a provoqué un plantage. Aucun test pour vérifier que l’ajout de cette entrée existait donc ce bug est passé en production, or une simple vérification de la limite d’entrées aurait suffit pour s’apercevoir de l'existence du bug.

Ce bug a provoqué la chute de l’action de l’entreprise en 12 jours de 342 euros à 234 euros soit une perte de valorisation de 25 milliards. De nombreux clients utilisant windows serveurs comme les hôpitaux qui ont reporté des actes médicaux ou les compagnies aériennes comme Delta Ed Bastion ont perdu 500 millions de dollars et on dû annuler pour tous les états unis 3000 vols intérieurs et 11 000 retardés.

## Apache Commons

Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

Avec InvokerTransformer sérializable, du code java arbitraire peut être exécuté en injectant des objets sérialisés avec des end points acceptant du java sérialisé comme JMX, RMI, remote EJB,...
Pour résoudre ce bug, de nombreuses applications pourraient s’en trouver affectées et ne plus fonctionner ce qui fait que ce bug est global
Ce bug met donc aussi toute la librairie en danger d'être refusée pour ceux qui ont une exigence en terme de sécurité.
La solution proposée a été de proposer l’option d’accepter ou non que l’InvokerTransformer peut être sérialisé ou non.
Le bug a été découvert avec des utilisateurs et aucun test n’a été créé pour vérifier.

## Netflix Chaos Engineering

Netflix is famous, among other things we love, for the popularization of _Chaos Engineering_, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

## 1. Expériences concrètes réalisées

Netflix utilise des outils comme Chaos Monkey, qui désactive aléatoirement des instances de machines virtuelles dans le cloud AWS pour tester la robustesse des services. Chaos Kong simule des pannes à grande échelle, comme la défaillance d'une région entière d'AWS. Par ailleurs, Failure Injection Testing (FIT) introduit des latences, des échecs de requêtes, ou des interruptions de services spécifiques. Ces expériences aident à valider que les systèmes peuvent dégrader leurs services de manière contrôlée et éviter les interruptions globales.

## 2. Exigences pour ces expériences

Ces expériences nécessitent :

-   Un environnement distribué en production, idéalement basé sur des infrastructures comme AWS.
-   Des outils spécifiques pour injecter des défaillances ou simuler des scénarios de panne.
-   Des mécanismes pour surveiller l'état du système en temps réel.
-   Une culture organisationnelle prête à tolérer les risques temporaires pour améliorer la résilience à long terme.

## 3. Variables observées

Les variables clés incluent des métriques reflétant le comportement de l'utilisateur :

-   Stream starts per second (SPS) : Taux de démarrage des flux, indicateur principal de la disponibilité.
-   Latence des requêtes et taux d'erreurs : Pour détecter des impacts internes avant qu'ils n'affectent les utilisateurs.
-   Dégradation contrôlée des services : Par exemple, si un système de recommandations échoue, Netflix affiche des titres populaires comme solution de secours.

## 4. Résultats principaux obtenus

Ces approches ont permis à Netflix :

-   D’augmenter la résilience de ses systèmes face aux défaillances inattendues.
-   De renforcer une culture d'ingénierie qui priorise la robustesse.
-   D’identifier et de résoudre des faiblesses structurelles, tout en s’assurant que les interruptions d’un service n’affectent pas l'expérience globale de l'utilisateur.

## 5. Netflix est-elle seule à réaliser ces expériences ?

Non. Amazon, Google, Microsoft et Facebook pratiquent également des approches similaires. Par exemple, Amazon injecte des pannes réseau pour évaluer la robustesse de ses services, et Facebook a simulé des coupures complètes de datacenters pour tester la tolérance aux pannes de ses infrastructures.

## 6. Applications dans d’autres organisations

Les principes d’ingénierie du chaos peuvent s’étendre à divers domaines comme l'E-commerce, de façon à simuler des pannes de bases de données pour évaluer l'impact sur le traitement des transactions. Dans le domaine de la banque pour tester la résilience des systèmes de paiement sous des charges élevées ou encore dans le domaine la santé en vérifiant la disponibilité des données critiques en cas de pannes réseau.

Les variables à observer pourraient inclure le nombre de transactions réussies, le temps de réponse des systèmes critiques, ou les taux d'erreurs sous charge.

## WebAssembly

[WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested?

Les avantages d’avoir fait une spécification pour le langage sont la connaissance précise du comportement du langage dans tous les différents cas.
Les avantages de faire une spécification
Grâce à celle-ci il est également possible de garantir un certain niveau de sécurité au niveau du typage et de la sécurité mémoire. Cela permet également de diminuer de potentiels ambiguïtés pendant la phase de développement en garantissant une structure logique et propre pour le langage.Il est également possible d’utiliser des analyseurs statiques et des compilateurs dérivant directement de la spécification, ce qui garantira un certain standard pour les développeurs.
Pourquoi il faut quand même faire des tests
Selon nous, le fait que webAssembly a été formellement spécifié ne garantit pas qu’il est sans bug car sa spécification est théoriquement correcte mais il faut aussi vérifier que la théorie fonctionne dans le monde réel. Il peut également y avoir des bugs potentiels sur certains navigateurs ou systèmes que la théorie n’aura pas prévu. Il peut également avoir des problèmes de performance qui ne peuvent être testés que dans le monde réel. Des tests permettront également de garantir qu’à l’avenir que des régressions ne seront pas commises après l’ajout de nouvelles fonctionnalités.

## Isabelle

Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

### Résumé de l'article :

Cet article présente une spécification mécanisée du langage WebAssembly, réalisée dans l'outil formel Isabelle. Cette spécification mécanisée propose une sémantique formelle de WebAssembly et inclut des preuves mécanisées de la solidité du système de types du langage. Elle s'accompagne d'un interpréteur exécutable vérifié et d'un vérificateur de types. Les auteurs ont utilisé cette approche pour identifier et corriger plusieurs problèmes dans la spécification officielle de WebAssembly, influençant son développement. Enfin, l'article détaille comment cette spécification a été validée via des tests différentiels et des conforment tests du langage.

### Réponses aux questions :

#### 1. Avantages de la spécification mécanisée

Dans l'article, l'outil Isabelle est utilisé afin d'obtenir une spécification plus fiable, en prouvant formellement des propriétés comme la solidité du système de types par exemple.  ce dernier a permis en plus d’identifier et de corriger des failles présentes dans la spécification originale, y compris des erreurs qui auraient pu conduire à des comportements non prévus voir même dangereux.

La spécification mécanisée est plus fiable. Elle permet de prouver des propriétés importantes, comme par exemple la solidité du système de types qui n'étaient pas démontrées dans les versions écrites manuelles. Grâce à l'outil Isabelle, elle a aussi permis d'identifier et de corriger des erreurs dans la spécification originale.

#### 2. Améliorations apportées à la spécification formelle originale

Oui, la spécification mécanisée a permis d'améliorer l'originale en corrigeant plusieurs défauts comme la propagation d'exceptions, les opérations de retour et les fonctions hôtes.

La propagation d'exceptions a été corrigée pour éviter des erreurs dans la gestion des exceptions.
La sémantique de l'instruction `Return` a été révisée pour éviter des comportements mal définis.
Les contraintes sur les fonctions hôtes ont été renforcées pour éviter des erreurs dans l'état d'ex

#### 3. Autres artefacts dérivés de la spécification mécanisée

Les artefacts principaux dérivés incluent :

-   Interpréteur exécutable vérifié : Implémenté comme une fonction Isabelle et prouvé correct par rapport à la spécification mécanisée.
-   Vérificateur de types exécutables : Conçu pour fonctionner en une seule passe sur le programme et prouvé correct.

#### 4. Vérification de la spécification par l’auteur

Les propriétés fondamentales du langage WebAssembly, progress et préservation, ont été démontrées formellement par induction sur la sémantique mécanisée du langage. Ces propriétés garantissent que :

-   Chaque configuration bien typée évolue soit vers une autre configuration bien typée, soit atteint un état terminal (propriété de progress).
-   Les transformations exécutées pendant l’exécution n’altèrent pas la validité des types initialement déclarés (propriété de préservation).  
    Ces preuves nécessitent l’établissement de nombreux lemmes auxiliaires pour des cas complexes, notamment liés à des structures d'exécution imbriquées ou à des instructions comme `br` et `Return`.

L’interpréteur dérivé de la spécification mécanisée a été évalué avec la suite de tests de conformité officielle de WebAssembly, qui vérifie la compatibilité des implémentations avec les spécifications du langage. Ces tests validés démontrent que l’interpréteur suit strictement la sémantique officielle et couvre un large éventail de cas.

Des tests différentiels ont été réalisés pour comparer l'interpréteur mécanisé avec des implémentations majeures de moteurs WebAssembly (tels que ceux de navigateurs). Des outils comme CSmith et Binaryen ont été utilisés pour générer des cas de test automatisés à partir de programmes en C traduits en WebAssembly. Cette approche a non seulement validé l’interpréteur, mais a aussi permis de détecter des bogues dans des outils tiers comme le compilateur Binaryen, montrant la pertinence des tests systématiques même dans des écosystèmes matures.

#### 5. La nouvelle spécification rend-elle les tests inutiles ?

Non, la spécification mécanisée ne remplace pas les tests :

-   Elle garantit une correspondance correcte entre la sémantique formelle et les preuves, mais les tests sont essentiels pour valider les implémentations concrètes.
-   Les tests permettent également d’identifier des erreurs dans des outils auxiliaires comme les compilateurs ou générateurs de code.

#### 6. Spéculation sur d'autres applications

Dans d’autres organisations, des mécanisations similaires pourraient être réalisées pour :

-   Langages de programmation spécifiques : Pour prouver la solidité ou détecter des incohérences dans les systèmes de types.
-   Logiciels critiques : Comme les systèmes embarqués dans l’automobile ou l’aviation.
    Les variables observées incluraient la cohérence de l’état interne, la gestion des erreurs, ou l’interaction avec les composants externes. Ces mécanisations pourraient également faciliter la validation des implémentations par des tests automatisés.
