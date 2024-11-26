# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.



4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

## Question 1 :

Nous avons choisi un bug de l'application "WhatsApp for Android", concernant l'ajout de filtres sur des photos.
Un chercheur de l'entreprise "Check Point" a reverse engineerer la librarie  **libwhatsapp.so** et découvert que la méthode `applyFilterIntoBuffer()` 
s'occupant d'appliquer des filtres à une image chargée dans un buffer, faisait implicitement l'hypothèse que la taille et le format (RGBA) de l'image 
et du filtre était les mêmes. Ce bug introduit une faille de type "out-of-bounds read/write", rendant possible avec une image avec un format 
différent (en l'occurence ici un byte par pixel au lieu de 4) de faire copier dans le buffer l'équivalent de 4 fois sa taille de donnée. 
Ce bug aurait pu être évité en réalisant les bons scénarios de tests. Cette méthode faisant l'hypothèse que l'image était forcément au bon format. Un ou plusieurs cas de tests réalisés avec des images au mauvais format auraient permis de détecter le problème.
Avec de meilleures spécifications, le bug aurait également pu être évité grâce à de la programmation défensive, permettant d'assurer que l'hypothèse que l'image était au bon format. Ce qui a finalement été ajouté car "WhatsApp" a indiqué dans son communiqué avoir rajouté deux nouveaux checks dans cette même méthode.


## Question 2 :

Nous avons sélectionné [ce ticket](https://issues.apache.org/jira/browse/COLLECTIONS-796) qui mentionne un bug dans la méthode statique createSetBasedOnList appartenant à la class SetUniqueList. Le comportement obtenu après exécution de la méthode n'est pas celui attendu d'après la description, le set d'élément retourné ne contenant pas les valeurs de la liste passée en paramètre. Ce bug local provenait d'une ligne supprimée lors d'un commit précédent, ligne ajoutant tout les élements de la liste passée en paramètre dans le set retourné. La solution fut de rajouter cette ligne, et de fournir une méthode de test pour tester cette méthode statique (on notera au passage qu'il n'y avait pas de cas de test pour cette méthode). On peut aussi noter la résolution d'une typo dans la même [PR](https://github.com/apache/commons-collections/pull/255).

## Question 3 :

Netflix est une entreprise qui fournit des services à travers Internet. Afin d'accroître la valeur de leurs services, les ingénieurs travaillant à Netflix produisent tous les jours du nouveau code et des modifications.

C'est pourquoi Netflix se doit de réaliser des tests d'intégrations. De par son envergure, il est impossible de reproduire son architecture dans un environnement de tests contrôlé.

Netflix va alors au travers de trois types d'expérience appliquer une approche de "Chaos Engineering".

- La première **"Chaos Monkey"** va interrompre aléatoirement des instances de machines virtuelles pour vérifier que les services peuvent supporter des pannes.

- Une deuxième expérience **"Chaos Kong"** est un exercice qui va simuler l'échec d'une région entière d'Amazon EC2.

- Et la dernière **"Failure Injection Testing (FIT)"** vise à provoquer des échecs dans les requêtes entre services pour tester la dégradation contrôlée du système.


Pour réaliser à bien ces expériences, il faut tout d'abord définir un **"steady state"**, un indicateur permettant de s'assurer que le système fonctionne correctement.
Dans le cas de Netflix, il a été choisit le "Stream per second" (SPS) représentant le nombre d'utilisateurs qui lancent un film ou une série en 
streaming par seconde.

Ainsi on se concentre seulement sur l'expérience utilisateurs. Et dans le cas où un service non critique échoue, il se peut qu'il n'ait pas nécessairement d'impact sur la disponibilité globale du système, comme le service "Bookmark" de Netflix.

Ces expériences sont réalisées en production sur des **sous-ensembles d'utilisateurs**. Par exemple, on peut former deux groupes : un groupe confronté à un problème lié à l'expérience et un autre groupe sans problème. Cela permet de comparer les résultats et de détecter des erreurs éventuelles.

Enfin, pour s'assurer de la pertinence des résultats obtenus par ces tests. Netflix applique le 4ème principe du "Chaos engineering" : l'automatisation des tests.
Par exemple, des tests "Chaos monkey" sont réalisés continuellement tandis que les tests "Chaos Kong" sont réalisés moins fréquemment (ex : une fois par mois).

Bien que Netflix soit l'une des seules entreprises ayant documenté leur application du Chaos engineering, d'autres grandes entreprises du secteur appliquent les mêmes méthodes.
On pourrait, par exemple, pour entreprise comme Amazon, imaginer une application des mêmes méthodes en utilisant le nombre d'achats effectués par 
seconde en guise de "steady state".

## Question 4 :

Du fait de la construction du language il est possible d'effectué un certain nombre de vérifications statiquement, notamment sur la mémoire "memory accesses in WebAssembly can be guaranteed safe with a single dynamic bounds check." page 12 de l'article, où encore l'absence prouvée (entendre par là mathématiquement) de comportements inattendus lors de l'exécution de chaque instructions. Par exemple l'instruction i32.add, d'après la spécification garantit en résultat la somme des deux nombres valide, si ceux ci sont valides. Si le résultat dépasse un entier 32bit, alors la spécification indique que le résultat attendu sera précis et prévisible. Un autre exemple serait celui de la division par 0. La validation du code reçu est également simplifiée, de par la grammaire du language. Celà dit apparement (cf section 6) trois comportements d'implémentations peuvent être considérée comme toujours non déterministe. Concernant les différentes implémentations du support du language WebAssembly dans les différents navigateurs existant, il est bien entendu évident qu'il faut les tester, car bien que la spécification du language est prouvée, l'implémentation elle ne l'est pas.

## Question 5 :

Le papier relate la formalisation en utilisant Isabelle de la sémantique et des types du language WebAssembly, ainsi que la preuve de la robustesse de ces types. Il est mentionné que, afin de compléter cette preuve, il a fallut corriger certains aspect de la spécification de WebAssembly. Par exemple, la façon dont le language WebAssembly opérait avec son système d'exécution n'était pas formellement spécifié, et la formalisation par les auteurs de ce papier de ce système (avec Isabelle) a mené à la découverte d'un manque de robustesse dans le système de type de WebAssembly. Cependant celà n'empêche pas l'utilisation de test car bien que ce qui est prouvé mathématiquement est vrai rien n'empêche une erreur de postulat / d'assertion par exemple. Également en cours Benoît Combemale nous a parlé d'un cas similaire (je ne sais plus si il s'agissait de ce papier) ou des tests ont mis en évidences des problèmes sur un programme qui avait justement été prouvé auparavant avec Isabelle. L'article explique ensuite plus en détail du checker de type qu'ils ont définit à l'aide d'Isabelle, puis codé et compilé en exécutable, ainsi que l'interpreter qu'ils ont aussi conçu.




