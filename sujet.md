# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Le bug, de l'an 2000 est un bug qui est arrivé lors du passage à l'an 2000, en effet les dates sur les ordinateur étaient codées sur 2 chiffres (98 pour 1998). Donc quand est arrivé l'année 2000 le 19 n'était pas programmer pour passer à 20. C'est un  bug global qui a causer des gros problemes. Tous les ordinateurs ont du etre mise à jour. L'erreur vient du fait du manque de projetction à long terme. En effet le passage entre 1999 et 2000 était inévitable et aurai du etre anticiper, et s'il avait été anticipé, on aurait pu tester la reaction des programmes au changment vers l'année 2000.

src: https://www.bbvaopenmind.com/en/technology/innovation/the-5-most-infamous-software-bugs-in-history/

2. Ce bug est local, il ne provoque pas derreur au lancement, l'erreur vient d'une ligne qui a été supprimé par inadvertance et donc la valeur n'était plus bonne. Cette erreur aurai pu être detécté avant d'y être confronté si l'on avait tester cette methode.

src: https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-796?filter=doneissues

3. 
what are the concrete experiments they perform, 
regarder le comportement de leur systeme en cas de "real world inputs" comment des problemes de réseau, trop de demande du service en meme temps, et des données(films) mal formé, corrompu etc. Ils vont donc provoquer ces scénario volontairement pour comprendre la réaction de leur systeme.

what are the requirements for these experiments,
il faut provoquer ces situation sur leur service en production, pas sur un serveur "bac a sable" isolé, et a des horaires classique, pour qu'une personne puisse résoudre les bugs si jamais le service réagit mal à ces tests.

what are the variables they observe
"real-world events", comment l'arret d'une instance d'une machine virtuelle, un taux de latence des requete entre services qui augmente d'un coup, des requetes qui échoue entre les services, et meme un service qui crash
lobjectif est de tester leur materiel et leurs logiciels.


what are the main results they obtained. 
les résultats correspondent souvent au hypothèse, c'est a dire des crash coté client, comme par exemple un serveur surchargé qui va faire disparaitre le client de la file d'attente.
Donc on se rend compte que le service est encore exposé à des bugs

Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.
En effet netflix provoque des bugs tres ciblé et dans un cas extreme, mais cela permet d'anticiper et de corriger ces bugs avant qu'il ne puisse se produire. Car ce sont des bugs majeurs.
Le chaos engineering est donc une nouvelle facon de tester un service ou un programme. A partir du moment ou l'on fourni un service sur internet, peut importe le domaine, la menace d'un probleme de connexion ou de surchage existe, c'est pour quoi je pense qu'il est important de prendre en considération ce genre de test, quelque soit le logiciel que l'on developpe.

4. 
Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. 
Rapide, portable, compacte et sûr.

In your opinion, does this mean that WebAssembly implementations should not be tested? 
A mon avis, puisque l'on code en bas niveau on est plus au courant du comportement de notre programme, comparé à du java qui lui meme fait la conversion en langage bas level, c'est pourquoi on est tenté de ne pas tester son code et se faire confiance. De plus les fonction de qui permettent de tester un code sont majoritaire en langage haut niveau.
Mais je pense qu'il est important de quand meme tester du code bas niveau car l'erreur est humaine et meme si il y a moins de bug algorithme, une faute de frappe ou une erreur d'étourderie peut entrainer un bug qui peut engendrer plus ou moins de conséquences.

5. 
what are the main advantages of the mechanized specification? 
Cela permet de tester et comprendre le comportement de notre programme

Did it help improving the original formal specification of the language? 
Cela permet de tester la validité algorithme mais elle n'empeche pas l'erreur humaine.

What other artifacts were derived from this mechanized specification? 
type soundness
type checker and interpreter

How did the author verify the specification? 
par démonstration par induction

Does this new specification removes the need for testing?
non, cela fait parti des test necessaire et permet de corriger des bugs qui ne seront detecter que par cette technique, mais il peut rester des zones dombres si on utilise seulement cette methode.
Les test doivent être variés, que ce soit par les exemple ou par les méthodes de test.
