# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1.  Google a récemment publié un article à propos de l'utilisation de l'IA dans la recherche de vulnérabilités.
En utilisant cette technique, ils ont pu identifier 26 bugs, dont un concernant Open SSL, une librairie de cryptographie très largement utilisée dans le monde.
Lors de l'utilisation de fonctions de son API permettant la "manipulation de courbes elliptiques" parfois utilisées en cryptographie, et en renseignant de paramètres qui ne sont pas fiables, on peut lire ou écrire hors des limites de la mémoire.
Cela cause un crash et peut aussi théoriquement être utilisé pour injecter du code malveillant.
Malgré tout, ce bug reste mineur et local, car ces fonctions mathématiques sont très peu utilisées, et donc concernent une faible des utilisateurs d'Open SSL.
De plus, les principaux protocoles de cryptographie n'autorisent pas l'utilisation de valeurs non fiables pour ces fonctions, et empêchent que des valeurs problématiques soient utilisées avec Open SSL.
Sur une librairie aussi largement utilisée que Open SSL il est difficile de dire si un meilleur scénario de testing aurait pu le détecter plus tôt.
Je pense néanmoins que des tests de son API en mode boîte noire en injectant des valeurs hors du cadre d'utilisation prévu auraient peut-être pu le détecter.

Lien vers l'article[https://security.googleblog.com/2024/11/leveling-up-fuzzing-finding-more.html]

2.  Ce bug est un bug mineur et local à une fonction en particulier.
Il a été découvert que la fonction nommée "createSetBasedOnList" doit ajouter une liste à la valeur rendue, mais ne le fait plus.
C'est une erreur humaine de suppression d'une ligne dans le code, qui n'a nécessité que la réécriture de cette ligne.
J'ai vu dans la purposal to merge que la personne ayant corrigé le bug en a profité pour remettre le code au propre et renommer certaines fonctions qui avaient une faute d'orthographe.
Il a aussi ajouté un test et adapté les existants a ses corrections dans le code.

Lien vers le ticket Jira du bug[https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-796?filter=doneissues]

3.  La pratique du "Chaos Engineering" est une nouvelle discipline dans l'industrie.
Pour mettre en place cette technique de test, Netflix utilise ce qu'ils appellent le "Chaos Monkey", 
qui durant les heures de travail des ingénieurs choisit aléatoirement un serveur et l'arrête.
Ils mènent aussi des exercices avec le "Chaos Kong" qui lui simule carrément l'arrêt de régions entières du cloud Amazon.
Le design de ces expériences nécessite l'application de quatre principes :
 - Construire une hypothèse autour d'un état stable du système
 - Faire varier les événements
 - Conduire les expériences en production
 - Automatiser ces expériences pour les lancer continuellement

Pour mesurer ces expériences Netflix utilise une unité nommée "SPS", pour "starts (= stream) per second".
Elle correspond au nombre d'utilisateurs qui lancent un flux à chaque seconde.
Pour les expériences de Chaos Engineering, ils n'utilisent pas de métriques plus précises comme la charge CPU ou le temps de réponse en base de données.
En effet, le but ici est de tester directement l'interaction entre les utilisateurs et le système.
Pour eux, tant que le service est opérationnel, et même si il est dégradé, la mission est remplie.
A chaque exercice de Chaos Engineering, deux résultats sont possible: soit le système a tenu et a prouvé sa résilience, soit une faiblesse a été découverte et suggère une piste d'amélioration.
Netflix ne sont pas les seuls à utiliser le Chaos Engineering, ils ont remarqué l'utilisation de principes de test de systèmes similaires chez Google, Amazon, Microsoft et Facebook.

4.  Les avantages principaux d'une spécification formelle en Web Assembly sont :
 - Précision et clarté : fournir aux développeurs une documentation claire et précise.
 - Consistance dans l'implémentation : permettre à des implémentations complètement indépendantes de se comporter de la même manière.
 - Exactitude : ses propriétés peuvent être démontrées mathématiquement.
 - Sécurité : il est facile de détecter des erreurs (mémoire, contraintes, vulnérabilités).
 - Portabilité : on peut faire du WebAssembly facilement sur une très grande variété d'environnements.

Malgré ces points forts, je ne vois pas le rapport entre les avantages d'une spécification formelle et le fait de ne plus avoir besoin de tests dans les implémentations de cette spécification.
Les bugs sont des phénomènes émergents qui se manifestent dans l'interaction des règles et des composants entre eux.
Plus on produit quelque chose de complexe et plus on a de chances que des interactions non prévues apparaissent entre les composants.
Donc, non, il faut toujours tester pour pouvoir assurer du mieux possible que les implémentations fonctionnent de la manière attendue.

5.  D'après l'auteur, les deux principaux avantages de la spécification mécanisée sont :
 - Avoir une spécification propice à la conduction de preuves.
 - Avoir un interpréteur capable d'optimisation.

Il conclut son article en expliquant cette spécification est faite à la main, diffère beaucoup de la spécification originelle, ne contient aucun support d'interaction avec son environnement, et n'offre aucune preuve de respecter la propriété de correction du WebAssembly classique.
Il explique qu'a leur connaissance, Isabelle représente le premier travail de méchanisation du WebAssembly.
Ici, on est dans un nouveau paradigme, où on fait de la programmation orientée par la preuve.
On peut donc techniquement prouver au sens mathématique qu'une implémentation fait ce qu'elle doit faire, et ne fais pas ce qu'elle ne doit pas faire.
Est-ce que cela suffit à dire qu'on peut se passer de tests ? J'avoue qu'il est très tentant de dire oui, mais en vérité, je ne sais pas.