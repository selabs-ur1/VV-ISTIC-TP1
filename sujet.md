# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1.  https://openai.com/index/march-20-chatgpt-outage/
  Le bug est apparu sur la plateforme ChatGPT le 20 mars 2024. Le bug est global puisqu'il impacte tous les utilisateurs qui se sont rendus dans l'onglet "Manage my subscriptions". Le bug permettait aux utilisateurs de voir les titres des conversations nouvellement créées des autres utilisateur. Le utilisateurs de chatGPT plus pouvaient également avoir accès aux données d'autres utilisateurs dans l'onglet "Manage my subscriptions" (nom, email, adresse, type de carte, 4 derniers numéros de la carte de paiement).
Ce bug a été causé par un bug dans la librairie opensource redis qui permet de gérer le cache.
Les répercussions sont des fuites de données sensibles, ce qui nuit aux utilisateurs et à l'image de l'entreprise.
Le bug a été corrigé directement sur la librairie fautive.
Des tests de mise à l'échelle auraient certainement permis de découvrir ce bug en mettant en place des scénarios avec beaucoup d'utilisateurs simultanés, cependant ce type de tests peut être compliqué à mettre en place.

2.  https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-842?filter=doneissues

New JDK 21 method in java.util.List addLast has a void return type. This clashes with the existing boolean return from the same method in AbstractLinkedList.

C'est un bug global qui peu affecter n'importe qui utilisant la librairie apache.
   
Solution : Deprecated existing AbstractLinkedList (and sub-classes) and added a replacement list compatible with Java 21.

Aucune mention de tests, la fonctionnalité est dépréciée.


3.  Expériences menées :
    * terminate virtual machine instances
    * inject latency into requests between services
    * fail requests between services
    * fail an internal service
    * make an entire Amazon region unavailable
    
    Exigences pour les expériences :
    * Définir un état stable (SPS)
    * Hypothèse : Cet état stable persiste malgré les perturbations.
    * Stimulation 
    * Observation 
    
    Indicateurs observés :
    * SPS (Stream start Per Second), s'il diminue c'est que le système n'a pas su géré l'erreur introduite.
    * Indicateurs à l’échelle interne : Temps de réponse, latence, ou consommation CPU pour détecter les dégradations invisibles pour l’utilisateur.

    Le Chaos Engineering est une technique également utilisée chez Google, Amazon, Facebook et Microsoft 

    Domaines possibles :
    * Banques et finance : Simuler des interruptions dans les échanges de données ou des erreurs de traitement et observer des délais dans les transactions.
    * E-commerce : Simuler des pics de trafic soudains ou des pannes de paiement, oberver le trafic, des abandons de panier et des latences.
  
4.  La spécification formelle de WebAssembly garantit une définition précise et uniforme du langage, renforce la sécurité grâce à une vérification rigoureuse, et facilite l’interopérabilité entre différents environnements. Elle permet également aux développeurs de compiler et d'optimiser en respectant des règles bien précises.
  Les tests des implémentations restent essentiels pour détecter les erreurs de codage, vérifier l’interopérabilité, simuler des scénarios réels, et évaluer les performances, aspects non couverts par la spécification.

5.  Les avantages de la spécification mécanisée de WebAssembly est l'absence d'ambiguïtés grâce à l'abstraction des comportements arithmétiques et de la mémoire, ce qui permet une meilleure clarté et une preuve formelle de la validité du système.
   Cela a permis d'améliorer la spécification formelle d'origine en identifiant et corrigeant plusieurs erreurs, comme des problèmes de propagation des exceptions et d'opérations de retour incorrectes.
Cela introduit un vérificateur de types et un interpréteur exécutable.
Cette spécification ne remplace pas les tests, elle les complète, car des tests de conformité restent nécessaires pour valider l'interprétation du langage.
