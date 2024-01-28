Samuel SOUTREL

Baptiste MEAUZÉ


# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

## Énoncé 1 : 

*Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.*

## Réponse : 

Le Therac-25 était une machine de radiothérapi
par la société canadienne AECL (Atomic Energy of Canada Limited). Il permetait de réaliser des traitements de radiothérapie contre le cancer à l'aide des rayonnements.
Le bug dans le Therac-25 a été causé par des erreurs de conception logicielle dans le système de contrôle du faisceau de radiation. 
Le dispositif utilisait un logiciel pour contrôler la délivrance de rayonnement lors des traitements de radiothérapie. 
Le bug était une race condition, donc un bug global, une situation où le comportement du système dépendait de l'ordre dans lequel des événements concurrents se produisaient.
Ce bug se manifestait lorsqu'un opérateur changeait rapidement entre deux modes de traitement différents.
Si l'opérateur effectuait cette transition trop rapidement, 
le logiciel pouvait entrer dans un état où il ne pouvait pas garantir correctement la sécurité du traitement. 
Cela pouvait entraîner la délivrance de doses de rayonnement excessives, 
parfois des milliers de fois plus élevées que la dose prescrite.
6 accidents ont eu lieux en 2 ans, dont 3 morts. 
Aucun test n'avais été réalisés et ces accidents auraient certainement pu être évités si des tests avaient été fait.

Sources : *https://users.csc.calpoly.edu/~jdalbey/SWE/Papers/THERAC25.html*
*https://fr.wikipedia.org/wiki/Therac-25#:~:text=Therac%2D25%20%C3%A9tait%20le%20nom,%2D6%20et%20Therac%2D20.*


## Énoncé 2 : 

*2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?*

## Réponse : 

Nous avons choisi l'anomalie : COLLECTIONS-796

URL : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-796?filter=doneissues

Assignee: Bruno P. Kinoshita

Reporter: Owen Mansel-Chan

Status: RESOLVED

Ce bogue est un bogue local et a été corrigé
Et effectivement un nouveau teste a été ajouté afin de prévenir de potentiel régression. *SetUniqueList.createSetBasedOnList* n'ajoute plus d'éléments de liste à la valeur de retour. 
La documentation dit que c'est le cas, et c'était le cas jusqu'à la version 4.2, 
et un appel à *addAll* a été accidentellement supprimé. Afin de résoudre ce problème il faut rajouter *subSet.addAll(list);*
à la fin de la méthode createSetBasedOnList();


## Énoncé 3 : 

*Netflix is famous, among other things we love, for the popularization of Chaos Engineering, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.*

## Réponse : 

Netflix utilise Chaos Monkey, un service interne, pour provoquer aléatoirement la terminaison d'instances 
de machines virtuelles hébergeant des services de production.
D'autres exercices, tels que "Chaos Kong" et des tests d'injection de défaillance, simulent des scénarios plus complexes, 
évaluant la robustesse du système face à des pannes régionales ou des défaillances de requêtes entre services.

Pour mener à bien ces expériences on doit définir quelques prerequis tel que : des hypothèses, des variables indépendantes et dépendantes 
et le contexte.
Ils observent : le temps de récupération, la latence, la taux d'erreur et la disponibilité.
L'ingénierie du chaos est adoptée également par des entreprises comme Amazon, Google, Microsoft et Facebook.
Ce concept pourrait être appliqué à d'autres organisations dépendant de systèmes distribués, 
adaptant les expériences aux architectures spécifiques pour assurer la robustesse opérationnelle.


## Énoncé 4 : 

*WebAssembly has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in a scientific paper published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested?*

## Réponse : 

L'usage d'une spécification formelle pour le WebAssembly offre une base solide pour le développement en assurant la cohérence 
et une compréhension claire des fonctionnalités. Elle facilite la communication entre les développeurs 
en définissant précisément le comportement attendu, réduisant les ambiguïtés et assurant une mise en œuvre uniforme.
Malgré la spécification formelle, il est tout de même crucial de tester rigoureusement les implémentations pour garantir leur conformité et ainsi repérer d'éventuels écarts et éviter l'introduction d'erreurs. Les tests demeurent essentiels pour assurer la fiabilité 
et la stabilité de WebAssembly dans des conditions d'utilisation réelles.

## Énoncé 5 : 

*Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?*

## Réponse : 


Vérification Formelle : La spécification est mécanisée à l'aide d'Isabelle, impliquant une vérification formelle du langage.

Interpréteur et Vérificateur de Type Vérifiés : Un interpréteur et un vérificateur de type vérifiés ont été créés, renforçant la fiabilité de l'implémentation par rapport à la spécification.
    
Preuve de Cohérence du Système de Types : La spécification mécanisée inclut une preuve formelle de la cohérence du système de types de WebAssembly.

Amélioration de la spécification formelle :
La mécanisation a permis d'identifier des problèmes dans la spécification officielle, influençant son développement.
Des corrections ont été apportées à la spécification officielle pour remédier aux lacunes exposées par la mécanisation.

Artefacts dérivés de la spécification mécanisée :
Un interpréteur et un vérificateur de type vérifiés ont été dérivés de la spécification mécanisée.
Des preuves formelles de la cohérence du système de types ont été générées.

Vérification de la spécification :
La spécification a été vérifiée à l'aide d'Isabelle, un assistant de preuve.
Une approche de validation expérimentale a été adoptée en utilisant l'interpréteur 
vérifié avec le test de conformité officiel de WebAssembly et des expériences de fuzzing.

Nécessité continue de tests :
Bien que la spécification mécanisée et la vérification formelle renforcent la fiabilité, des tests restent nécessaires.
L'auteur a effectué des expériences de fuzzing pour valider la conformité de l'implémentation par rapport au langage réel.