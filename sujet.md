# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If
   possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the
   repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate
   whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue
   tracking systems to discuss and follow the evolution of bugs and new features. The following
   link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the
   issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds
   to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the
   contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance
   verification technique. The company has implemented protocols to test their entire system in production by simulating
   faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering
   content under different conditions. The technique was described
   in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly
   explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the
   variables they observe and what are the main results they obtained. Is Netflix the only company performing these
   experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of
   experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The
   language was born from a joint effort of the major players in the Web. Its creators presented their design decisions
   and the formal specification
   in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf)
   published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web
   and other embedding environments. The authors say that it is the first industrial strength language designed with
   formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the
   paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion,
   does this mean that WebAssembly implementations should not be tested?

5. Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using
   Isabelle. The paper can be consulted
   here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This
   mechanized specification complements the first formalization attempt from the paper. According to the author of this
   second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal
   specification of the language? What other artifacts were derived from this mechanized specification? How did the
   author verify the specification? Does this new specification removes the need for testing?

## Answers

1. En 2021, un peu moins de 12 000 Tesla sont impactées par un bug pouvant avoir de graves conséquences. En effet, ce
   bug faisait freiner brusquement la voiture lorsque celle-ci était en mode conduite autonome. C'est un bug global, car
   il touche toutes les voitures Tesla qui ont la version 10.3. Le bug pouvait entrainer des accidents pour les
   conducteurs de Tesla. Un freinage brusque sans raison apparente peut effectivement causer des collisions de voiture
   suiveuse. Au niveau de l'entreprise Tesla, le bug les a contraints de rappeler toutes les voitures affectées par
   celui-ci. Ce bug survient à cause d’une déconnexion logicielle entre deux puces du véhicule, précisément quand le
   véhicule démarre après avoir été dans un mode spécifique, où les puces entrent en mode de veille pour
   économiser de l’énergie. À notre avis, ce bug aurait pu être évité si ce cas spécifique avait été testé.

2.  Nous avons décidé d'étudier le bug https://issues.apache.org/jira/browse/COLLECTIONS-709.
    Ce bug est local car il impacte uniquement la gestion des entrées dans les implémentations de MultiSet.

    Problème : Chaque élément d'un MultiSet est associé à un compteur. Le problème est que lorsque toutes les occurences d'un élément du MultiSet sont supprimés, le compteur de l'objet MultiSet.Entry n'est pas réinitialisé à zéro.

    Solution : La solution apportée est simple, il s'agissait juste d'un oubli de mettre la valeur du compteur dans l'entrée associée à zéro dans la méthode remove.

    Tests : Les contributeurs ont bien ajouté un test pour vérifier que le compteur est bien réinitialisé à zéro lorsque toutes les occurences d'un élément sont supprimées. Il vérifie bien dans le cas ou les occurences sont supprimées une par une.

3. À l'échelle de Netflix, les tests logicielles ne suffisent plus. Il faut tester la résistance des systèmes
   distribués. Pour ce faire, Netflix a conçu un certain nombre d'événements chaotique. Parmi ces évènements, on
   retrouve
   les suivants :

    - Chaos Monkey : Arrêt d'une VM de production
    - Chaos Kong : Simulation d'un arrêt complet d'une région Amazon Web Service
    - Ajout de latence entre les services
    - Simulation de panne de service interne

   Pour mesurer les résultats de leurs tests, Netflix utilise une donnée appelée SPS ((Stream) Starts Per Seconds).
   Netflix peut utiliser cette donnée, car il possède une énorme base d'utilisateur ce qui permet d'avoir des tendances
   de streaming très régulières. Le SPS issu des tests peut alors être comparés avec le SPS habituelle pour vérifier que
   tout fonctionne correctement.

   Netflix a observé des résultats particulièrement encourageant en appliquant ce genre de test. Les ingénieurs font en
   sorte que les services qu'ils produisent résistants aux différents évènements qui peuvent être injectés.

   D'autres entreprises telles que Google, Amazon, Microsoft ou Facebook pratiquent aussi maintenant le Chaos
   Engineering.

4.  Les avantages principaux d'avoir une spécification formelle pour WebAssembly sont les suivants :
    - Permet d'éviter les comportements imprévus car toutes les instructions sont définies de manière précise et respectent les mêmes règles.
    - Permet d'avoir une sécurité accrue car chaque règle de sécurité sont respectées à chaque fois.
    - Permet d'accélérer le processus de vérification car il est possible de valider le code de façon linéaire sans avoir à retourner en arrière.
    - Permet de faciliter l'ajout de nouvelles fonctionnalités de manière sécurisée et plus efficace.

    Avoir une spécification formelle ne signifie pas que les implémentations de WebAssembly ne doivent pas être testées.
    En effet, les tests permettent de vérifier que l'implémentation respecte bien la spécification formelle et permettent également de vérifier que l'implémentation est correcte et qu'elle ne contient pas de bugs.
    Les tests permettent également d'avoir un apperçu de la performance ainsi que de vérifier son comportement dans différents environnements ou matériels.

5. L'avantage principal de la spécification mécanisée avec le langage de preuve Isabelle est qu'elle permet d'avoir des
   preuves de bonne spécification contrairement à la spécification formelle originale. Cette spécification mécanisée à
   parmi de détecter des erreurs dans la spécification WebAssembly. Par exemple la propagation incorrect d'exception ou
   des règles insuffisantes de typage ont été corrigées. La spécification mécanisée a permis d'identifier des
   incohérences et de valider les implémentations.

   L'auteur a vérifié la spécification mécanisée de WebAssembly avec l'utilisation de ce qui est appelé dans l'article
   le Fuzzing. C'est une méthode qui consiste à effectuer des tests aléatoires sur l'interpréteur et sur des moteurs
   commerciaux de WebAssembly puis de les comparer.

   La spécification mécanisée ne supprime pas complètement le besoin de tests. Bien qu'elle améliore considérablement la
   confiance dans la solidité théorique de WebAssembly et des outils associés, les tests restent nécessaires. Il faut en
   effet tester les cas spécifiques comme l'intégration avec l'environnement hôte



