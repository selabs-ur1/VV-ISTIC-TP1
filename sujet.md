# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1) Source : https://www.nytimes.com/1986/06/21/us/fatal-radiation-dose-in-therapy-attributed-to-computer-mistake.html
   Le bug dans le logiciel Therac-25 était lié à l'administration du dosage de radiothérapie contrôlée par logiciel de l'appareil. L'appareil avait deux modes de fonctionnement : un mode faible puissance pour la thérapie par faisceaux d'électrons et un mode haute puissance pour la thérapie par rayons X. En raison d'une condition de concurrence critique dans le logiciel, la transition entre ces modes pourrait être déclenchée de manière incorrecte, ce qui amènerait la machine à délivrer un rayonnement d'une intensité beaucoup plus élevée que prévu, ce qui pourrait potentiellement tuer des personnes.

Il s’agissait d’un bug de concurrence global qui affectait toutes les unités Therac-25.

2) Source : https://github.com/apache/commons-imaging/pull/136/commits/d65ec98d3db20da2ea48a87b88c684d377988541

   C'est un bug local. Le code possède des additions à 0 et des multiplications à 0. Ce sont des opérations inutiles pouvant être simplifiées. 

   Les développeurs n'ont pas ajoutés de tests, il s'agit d'une suppression de code redondant. Les tests de non-regression doivent encore passer pour valider le changement.

3) Netflix possède plusieurs expériences pour tester la robustesse de leurs systèmes. Le Chaos Monkey consiste à simuler des pannes individuelles à l'aide de machines virtuelles. Le Chaos Kong consiste à simuler des pannes groupées en rendant une région entierement indisponible. Netflix utilise aussi l'injection de pannes comme des échecs de communications, requêtes, etc.

Ces tests ont pour objectif d'améliorer le service client ou de détecter des failles. Afin d'y arriver, ils enticipent les résultats de ces tests. 

Pour vérifier que le système fonctionne bien, Netflix utlise la métric SPS (Starts per second) qui analyse l'impact sur les utilisateurs en mesurant la disponibilité du contenu.

Les résultats sont un système robuste aux pannes, des failles détectées en amont ou une confiance accrue dans le système courant.

Netflix a mis en avant une méthode de tests à grande échelle et d'autres les ont pris comme exemple, LinkedIn ou les GAFAM par exemple. Discord pourrait utilisé cette technique afin de se rendre compte l'impact des problèmes serveurs qu'ils peuvent rencontrer. Identifier les causes des problèmes et rendre leur système plus fiable afin de contenter les utilisateurs.

4) 

