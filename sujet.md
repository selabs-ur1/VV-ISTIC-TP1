# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers
1. L'incident du sang vicié sur wow survenu du 13 septembre 2005 au 8 octobre 2005  est une pandémie virtuelle, elle a pris son origine dans une instance de donjon ou les joueurs pouvaient êtres infectés par un sort du boss final qui leur infligeait un débuff appelé "sang vicié" qui cause des dégâts au fil du temps.

Le débuff ce transmet entre personnages à proximité l'un de l'autre, à l'image d'une maladie contagieuse, et l'infection fait baisser les points de vie pendant un certain temps, tuant potentiellement les joueurs de faible niveau en quelques secondes.

En raison d'un bug, lorsque certains joueurs renvoyaient leurs familiers atteints par ce sort du "sang vicié", ceux-ci gardaient le debuff actif lorsqu'ils étaient invoqués à nouveau.

Seulement 3 serveurs on été touchés par cet incident, mais il a durant un temps provoqué de gros changements dans le comportement des joueurs. La mort n'étant pas définitive mais pénalisante beaucoup de joueurs désertais les grandes villes pour se retrouver dans les campagnes, moins fréquentées.

Blizzard le développeur a demandé aux joueurs de respecter des zones de quarantaine pour endiguer la maladie, mais cette stratégie a échoué : certains joueurs n'ont pas pris le risque au sérieux, tandis que d'autres ont profité de la situation. Les mesures de sécurité ont été contournées par certains joueurs. Par exemple, plusieurs ont volontairement contaminé leurs familiers.

Blizzard a été forcé de réinitialiser ses serveurs et d'appliquer des corrections logiciels rapides pour résoudre le problème4. Le problème s'est terminé le 8 octobre 2005, lorsque Blizzard a immunisé les familiers au sang vicié, ce qui a subséquemment empêché le sort de sortir de l'instance. 

Le code ayant provoqué ce bug n'est pas public, mais du fait qu'il est entièrement contenu dans un serveur de jeu on peut supposer qu'il est local.

Un test dans le bon scénario aurais totalement permis d'identifier le bug, on peut supposer que les développeurs avais testés l'instance en question avec des personnages ayant des familiers, mais le cas renvoyer un familier contaminé pour le rappeler une fois sortit n'a probablement pas été testé (le bug étant presque impossible a rater quand il se produit).

2. https://issues.apache.org/jira/browse/COLLECTIONS-278
Cette issue correspond a un problème de de méthodes put() et putall() qui n'update pas correctement un paramètre interne (une map en l'occurrence) qui est censée être renvoyé par un getter getKeys().
C'est un bug de type local puisqu'il est causé par une erreur de code dans la classe, les méthodes put ajoutent dans un paramètre et le getter en renvoie un autre.
Le contributeur a créer le ticket et proposé sa solution simple attribuant le contenu des put dans le bon paramètre, il a également écrit les tests correspondants.
Un autre contributeur a relu son travail et ajouter de la javadoc ainssi qu'un fix du même type sur le la méthode remove object.
Malheureusement cette contribution est rapidement devenue obsolète après sa validation car la classe en question a été supprimée.

3. Concrete experiments they perform
    - They have a service that terminates instances of production vms randomly
    - Simulation of entire amazon EC2 region down
    - Failure Injection Testing (FIT) to cause requests between services to fail and monitor the system response
    - Testing hardware failure
    - Testing unexspected surge in client requests
    - Testing malformed value in a runtime configuration parameter

what are the requirements for these experiments
    - A "steady state" of the system to measure a normal behavior
    - Hypotheses that this steady state will continue in the controll group and experimental
    - Independent variables
    - Dependent variables

what are the variables they observe
    Objective metrics like SPS (Stream Starts Per Second), new account sigups per second... Anything related to what they are testing that can be mesured in the test group and the control group.

what are the main results they obtained
    More confidence in their systems and in their unit tests

Is Netflix the only company performing these experiments?
    No Amazon, Google, Microsoft and Facebook are starting to do it too.

Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.
    Fun fact, je travaile pour le cloud d'orange en tant qu'automation specialist/devops, depuis 4 ans maintenant (quelques années chez OAB avant ça) donc je vais utiliser ce contexte et mon expérience pro pour spéculer.
    Du côté du cloud privé aucune chance de voir émerger ce type de tests, les clients décident de tout et refuseront en bloc de prendre le moindre risque, peut importe les arguments.
    Dans le cloud public les possibilités sont plus vastes et ce genre de tests pourrais totalement voir le jour.
    Je travaille justement a l'élaboration d'un système de monitoring des services bas niveau utilisés par l'api principale permettant aux clients (via un portail web) de commander de nouvelles ressources.
    Mettre en place ce genre de tests pour tester la résilience de l'ipmanager, du coffre fort de mot de passe ou d'un seul esx pour voir comment le vcenter ce débrouille pourrais être très intértessant.
    Mon travail consiste justement a remonter les métriques de disponibilités de ces outils que l'on interroge via API, on pourrais donc mesurer le taux d'acceptation des requètes (la charge n'est pas énorme, il faudrait en simuler), le pourcentage de perte de services réseaux en arrêtant les routeurs tier0 virtuels.
    (A mon niveau de scope mesurer la dispo d'une ou l'autre vm n'a pas grand sens)
    Spoiler alert, tout ça se passerais très mal, pour faire du chaos engineering il faut une insfrastructure construite un minimum pour la résilience, et pour des raisons de contraintes budgétaires et politiques ce cloud est designé avec en tête un fonctionnement nominal, le trop fort usage de l'over-provisionning causerais probablement des ravages dans la prod et nécéssiterais de lourdes adaptations avant de pouvoir commencer a êtres envisagées.
    (Mais promis je ferais lire ce papier au boulo, c'est prévu, on va bien ce marrer ^^)

4. What are the main advantages of having a formal specification for WebAssembly?
    Cela permet d'avoir un standard commun, compatible car poussé a l'adoption par le W3C et tout ces gros contributeurs.
    Avoir un cadre stric et bien défini permet d'éviter les bugs et les dérives, nottement en termes de sécurité puisque l'on parle de code pouvant être éxécuter sur les machines des utilisateurs qui consultent des sites web, il est important que l'implémentation d'un nouveau standard soit par exemple totalement isolé de l'environement sur lequel il tourne, pour éviter les failles de sécurités qui pourrait êtres exploitées pour accéder aux terminaux des utilisateurs

In your opinion, does this mean that WebAssembly implementations should not be tested?
    Bien sur, tout doit être testé, il faut déjà vérifier le respect de standard webassembly. 
    Mais avoir un cadre contraignant ne garantis pas que se cadre est sans faille et qu'il est impossible d'en sortir ou de créer des bugs dans ces limites.

5. What are the main advantages of the mechanized specification?
    Cela permet de définir les spécifications dans un language formel ce qui permet par la suite d'utiliser des outils pour les prouver et en vérifier directement la conformité de l'implémentation de façon automatique.
Did it help improving the original formal specification of the language?
    Oui car avant une spécification en language naturel ne pouvait être testée automatiquement.
How did the author verify the specification?
    Ils ont pu confirmer les spécifications grace a leur modèle dans isabelle
Does this new specification removes the need for testing?
    Non, on peut tester que ce qui a été spécifié a bien été implémenter, mais rien ne permet de dire que cette implémentation ne va pas planter a la première valeur pas super bien formatée.