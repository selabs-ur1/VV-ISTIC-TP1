# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

# 1] Bug fusée Ariane:

Le bug fait référence à une faille de conception du logiciel. Problème d’encodage : le logiciel de la fusée a utilisé un float de 64 bits pour représenter la vitesse qui a dépassé la limite pouvant être représentée par un entier sur 16 bits.

**Conséquences:**
- Pertes financières pour ArianeSpace, agence spatiale européenne
- Logiciels testés de façon plus rigoureuse et interconnectivité de l’application plus adaptée.

**Scénario pour prévoir le bug:**
Le bug aurait pu être évité en renforçant les tests et en faisant des tests dans des cas plus extrêmes.

# 2] [Lien vers le rapport JIRA](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-814?filter=doneissues)

Supprimer un objet vide d’une collection. Gestion d’erreurs : 
Le cas se produit lorsque l’on cherche à supprimer un élément vide d’une collection. Le programme ne lance pas de NPE si l’élément est vide mais uniquement s'il est null. On a donc anticipé ce cas pour gérer ce NPE.

# 3] Chaos Engineering

**Concrete Experiments:**
- **Chaos Monkey:** termine de manière aléatoire des instances de VM pour encourager les ingénieurs à concevoir des services qui supportent des défaillances individuelles.
- **Chaos Kong:** Simule la défaillance d'une région entière d'Amazon EC2 pour tester la résilience du système.
- **Exercices de Test d'Injection de pannes:** Injecte des pannes telles que des échecs de requêtes pour vérifier comment le système réagit.

**Prérequis:**
- Hypothèse sur la manière dont les événements du monde réel affectent le système.
- Tester dans l'environnement de production réel.
- Automatisation des tests.

**Variables:** SPS pour obtenir l’état de “santé” du système.

**Résultats:**
- Services capables de traiter les pires cas.
- Pratique devenue courante (Netflix fait office de modèle dans cette pratique).

**Spéculation:**
- Simulation d’autres types de défaillances (introduire des modifications aux paramètres d'exécution et observer le comportement du système).

# 4] Quels sont les principaux avantages d'avoir une spécification formelle pour WebAssembly?

Les principaux avantages sont : vitesse de validation, Vérifications des Limites, Amélioration du Temps de Compilation.

De notre point de vue, les tests sont cruciaux pour identifier d’éventuels bugs / failles de sécurité. Avec le temps des bugs peuvent émerger, et des tests peuvent servir à garantir la fiabilité de WebAssembly.

# 5] Quels sont les principaux avantages de la spécification mécanisée?
- Eyeball closeness : assure la correspondance entre spécification et mécanisation.
- Prise en compte des tests de conformité.

Cela a-t-il aidé à améliorer la spécification formelle originale du langage?
L'intégration d'une sémantique formelle dans la conception de WebAssembly a contribué à améliorer la spécification du langage, renforçant ainsi la fiabilité de ses caractéristiques comme sa spécification par exemple.

Comment l'auteur a-t-il vérifié la spécification?
L'auteur a vérifié la spécification de WebAssembly en utilisant l'assistant de preuve Isabelle.

Cette nouvelle spécification supprime-t-elle le besoin de tests?
Non, l’utilisation d’Isabelle n’enlève pas le besoin de tester son application. Isabelle ne va pas garantir l’absence d’erreurs dans une réelle implémentation.


