# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### 1 - Adrenalin 23.2.1 WHQL Driver

Un bug sur le driver de certaines carte graphiques AMD pouvait provoqué le crash de windows.
Lors de l'installation du driver, les utilisateurs peuvent sélectionner l'option "Factory reset", qui nécessite le redémarrage du pc pour finaliser l'installation. 
Ce redémarrage, si réalisé alors que Windows réalise de son côté des mises à jour en arrière plan, pouvait résulter en des fichiers systèmes windows corrompus chez certains utilisateurs.

Le bug est provoqué par l'arrêt forcé de windows pendant l'écriture sur des fichiers système.

Ce bug a eu comme répercussion, pour certaines personnes, la perte de données sur leur machine

On a dans ce cas un bug global.

Il a fallut aux ingénieurs d'AMD plusieurs essais avant de réussir à reproduire le bug, qu'ils qualifient comme très rare.

sources : 
* https://technewsspace.com/amd-confirms-its-graphics-driver-can-permanently-crash-windows-but-no-solution-yet/
* https://www.pcworld.com/article/1529986/rare-amd-radeon-driver-bug-corrupt-windows-fix.html

### 2 - COLLECTIONS-796 : SetUniqueList.createSetBasedOnList doesn't add list elements to return value

Ce bug est local.

D'après la documentation, SetUniqueList.createSetBasedOnList retourne les éléments de la liste, alors qu'il le faisait dans la version précédente.
L'erreur vient d'une ligne de code contenant "addAll" supprimée par inadvertance.
Elle a été corrigé en la rajoutant, et des tests ont été ajoutés pour s'assurer que la fonction retourne bien les éléments de la liste.

### 3 - Netflix : Chaos Engineering

Dans ce papier on peut retrouvé différentes expériences menées par Netflix pour tester ses systèmes.

On retrouve par exemple Chaos Kong, qui va simuler la défaillance d'une région entière d'Amazon EC2, ainsi que le Failure Injection Testing (FIT), qui va provoquer des échecs de requêtes entre les services Netflix.

L'une des exigences pour ces tests est qu'ils ne doivent être actif uniquement pendant les heures normales de travail (pour obtenir une réponse rapide des ingénieurs).

La variable principale observé pendant ces expérimentation est le SPS ((stream)Starts Per Second) qui correspond aux nombres de flux par seconde.

Netflix obtient grâce à ça des services plus stable, et des ingénieurs qui conçoivent leurs services pour gérer les défaillances d'instances.

Elle n'est pas la seule entreprise à utiliser ce type de tests, des entreprises comme Amazon, Google, Microsoft et Facebook le font également.

### 4 - WebAssembly

Les principaux avantages d'une spécification formelle pour WebAssembly sont :

* une spécification claire, ce qui permet une meilleure compréhension du fonctionnement de WA.
* la cohérence entre les différentes implémentations
* l'interopérabilité entre les implémentations
* la stabilité et fiabilité de la technologie

Ça ne rend pour autant pas les tests négligeables. Ceux-ci permettent d'identifier d'éventuels bug et ce qui va permettre de fiabilisé le système.

### 5 - Spécification mécanisée
