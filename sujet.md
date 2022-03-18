# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
### 1. Bug de l'an 2022

Des emails se sont retrouvés bloqués dans une file d'attente à cause du changement d'année en 2022. Le bug s'est manifesté sur les serveur Microsoft Exchange 2016 et 2019 au passage de l'année 2022. Il résulte de l'échec d'un date check. L'explication de Microsoft est la suivante : "The version checking performed against the signature file is causing the malware engine to crash, resulting in messages being stuck in transport queues.". La faille intervient lorsque l'antivirus tente de stocker la date dans une variable int32. Cette variable est limité à la valeur :2 147 483 647. Cependant, la valeur correspondant au 1er janvier 2022 à minuit est 2 201 010 001. Le bug est local et situé sur l'antivirus des serveurs d'email.
Tester avec des dates avancées aurait pu permettre la détection de ce bug.

Source :
[Le Monde Informatique](https://www.lemondeinformatique.fr/actualites/lire-microsoft-refait-le-coup-du-bug-de-l-an-2000-en-2022-dans-exchange-85294.html),
[Zdnet](https://www.zdnet.fr/actualites/bug-de-l-an-2022-pour-microsoft-exchange-39934815.html)

### 2. SetUniqueList.createSetBasedOnList doesn't add list elements to return value

[Fixed issue](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-796?filter=doneissues)


Il s'agit d'un bug local. La méthode permettant de créér un set à partir d'une liste ne fonctionne plus. Le correcteur explique que ce bug résulte de la suppression maladroite d'une ligne permettant d'ajouter les valeurs avant son return. 

La solution a été de réajouter cette ligne, le correcteur a également corrigé quelques typos au passage.
Il a également modifié les tests en place afin de s'assurer  de la taille de la liste. [corrrection](https://github.com/apache/commons-collections/pull/255/files)

### 3.Chaos engineering

Netflix introduit volontairement des turbulences dans son système afin de mettre à l'épreuve sa résistance aux échecs. Durant ces phases d'expérimentation, ils observent la stabilité du système en prennant pour KPI le SPS ((stream) Starts Per Second), qui observe le nombre de vidéos lancées par seconde. Cela pourrait également être le nombre de personnes se loggant à la seconde. Afin d'interpréter ces variables il faut les comparer à un état dit stable. Si les résultats sont assez proches, alors le système supporte bien les erreurs.
Netflix n'est pas la seule entreprise a éprouve le chaos engineering, les auteurs citent notamment les géants Amazon, Google, Microsoft et Facebook.

- Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

### 4.Web Assembly formal specifications
Les avantages à l'utilisation du WA sont nombreux. Il permet de réaliser des applications légères et portables proche du natif. Il est également penser de manière à être transparent sur le code source et les spécifications ce qui résulte en un débuggage facilité et une sécurité renforcée.

Quoiqu'il arriver Web Assembly devra être testé car il peut toujours y avaoir des erreurs dans l'implémentation.


### 5.Web Assembly et Isabelle
Les spécifications mécanisées permettent d'apporter une preuve du code. Cela permet de vérifier avec un exécutable l'interprétation et le typage.
Cette recherche a permis d'identifier et d'aider à corriger plusieurs erreurs dans le rapport officiel de spécifications WebAssembly. 
