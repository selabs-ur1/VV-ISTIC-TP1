# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

# Answers

## Authors
DESCHARREAUX Julian et CHAPLAIS Alexandre

1/- 

Description du bug :
Le bug de l'an 2000 était le résultat de programmeurs utilisant un code à deux chiffres pour représenter les années dans les champs de date, provoquant des problèmes lorsque l'année 2000 est arrivée. Les systèmes qui n'étaient pas conçus pour gérer le passage de "99" à "00" interprétaient l'année 2000 comme 1900, entraînant des calculs erronés, des erreurs et des défaillances du système. Le bug était global.

Manifestation de la défaillance :
La défaillance s'est manifestée de différentes manières selon les systèmes. Dans certains cas, les transactions financières étaient mal calculées, entraînant des soldes incorrects et des divergences financières. D'autres systèmes rencontraient des erreurs dans les fonctions dépendantes de la date, affectant la planification, la facturation et la gestion des dossiers.

Répercussions pour les clients/consommateurs et les entreprises :
Les clients et les consommateurs ont dû faire face à des défis tels que des relevés de facturation incorrects, des services perturbés et des pertes financières. Les entreprises utilisant un logiciel défectueux ont subi des dommages de réputation, des conséquences légales et des revers financiers en raison de la nécessité de correctifs d'urgence et de compensations.

Spéculation sur les scénarios de test :
Avec le recul, des tests approfondis axés sur les scénarios liés aux dates auraient pu aider à découvrir le bug de l'an 2000 plus tôt. Si les tests avaient impliqué des changements simulés de date vers l'an 2000, les développeurs auraient pu identifier et corriger les problèmes avant que le bug ne provoque des perturbations généralisées.

Conclusion :
Le bug de l'an 2000 est un exemple historique d'un problème logiciel mondial aux conséquences étendues. Il souligne l'importance de tests complets, en particulier dans des scénarios impliquant des calculs liés aux dates, pour éviter des défaillances généralisées et protéger les clients, les consommateurs et la réputation des entreprises impliquées.

2/-

L'issue COLLECTIONS-794 dans le projet Apache Commons Collections était une amélioration liée au code de test du filtre de Bloom. Le bogue consistait en des méthodes finales dans le code de test qui devaient être étendues pour de nouvelles implémentations, comme le filtre de Bloom stable. La résolution a été apportée dans la version 4.5, permettant l'extension nécessaire des méthodes. Il n'est pas mentionné dans le Github si dans les informations fournies si de nouveaux tests ont été ajoutés pour prévenir la réapparition du bogue.

3/- 

Netflix réalise des expériences, telles que les exercices "Chaos Kong" qui simulent la défaillance d'une région entière d'Amazon, ainsi que des tests d'injection de défaillance (FIT) où ils font échouer délibérément des requêtes entre les services Netflix pour vérifier la résilience du système.

Les expériences nécessitent une infrastructure capable de simuler des défaillances réelles, des mécanismes de sauvegarde pour minimiser les perturbations, et des protocoles de test rigoureux pour garantir la sécurité et la réactivité en cas de problème. Ensuite les variables observées incluent la disponibilité du service, le taux de démarrage de la diffusion de vidéos par seconde, et d'autres métriques liées au comportement du système.
Si les ingénieurs observent une variation inattendue dans SPS, nous savons qu'il y a un problème avec le système.
Netflix mentionne que des entreprises comme Amazon, Google, Microsoft et Facebook qui appliquent des techniques similaires pour tester la résilience de leurs systèmes.

D'autres organisations pourraient mener des expériences similaires en adaptant les scénarios de pannes aux caractéristiques spécifiques de leur infrastructure. Les variables observées dépendraient des priorités de chaque organisation, mais pourraient inclure la performance du réseau, la gestion des erreurs, et la résilience face à des charges inhabituelles.

4/-

Les principaux avantages d'avoir une spécification formelle pour WebAssembly, tels que décrits dans le texte, sont multiples. Tout d'abord, la spécification formelle garantit la sécurité, essentielle pour l'exécution de code provenant de sources non fiables sur le Web, en imposant la sécurité de la mémoire. De plus, elle assure l'efficacité en permettant une exécution rapide et optimisée du code, particulièrement crucial pour des applications web exigeantes telles que la visualisation 3D, l'audio, la vidéo et les jeux.

La portabilité est un autre avantage clé, avec la spécification formelle assurant l'indépendance par rapport aux langages, matériels et plates-formes, permettant ainsi une exécution cohérente sur divers environnements. La prévisibilité du comportement, rendue possible par la spécification formelle, facilite la compréhension et la prédiction du fonctionnement du code, contribuant à la fiabilité des applications web.

Malgré ces avantages, il est important de noter que la spécification formelle ne dispense pas des tests. Les tests demeurent essentiels pour valider la correction et la robustesse des implémentations réelles par rapport à la conception spécifiée. Cela garantit que WebAssembly fonctionne comme prévu sur différentes plateformes et navigateurs, en identifiant et corrigeant d'éventuels problèmes spécifiques à l'implémentation réelle.

5/- 

Les principaux avantages de la spécification mécanisée de WebAssembly sont les suivants:

Exécution Mécanisée et Vérification : L'auteur a créé une spécification mécanisée complète des sémantiques d'exécution de base et du système de types de WebAssembly. Cela inclut un interpréteur et un vérificateur de type vérifiés formellement.

Preuve de la Cohérence du Système de Types : Une preuve mécanisée de la cohérence du système de types de WebAssembly a été réalisée, renforçant la fiabilité du langage.

Identification et Correction de Déficiences dans la Spécification Officielle : Le travail sur la preuve de cohérence a révélé des lacunes dans la spécification officielle de WebAssembly, obligeant les auteurs à corriger certaines de ces déficiences pour garantir la cohérence du système de types.

Dialogue Constructif avec le Groupe de Travail : L'auteur a maintenu un dialogue constructif avec le groupe de travail de WebAssembly, contribuant à la spécification en mécanisant et vérifiant de nouvelles fonctionnalités au fur et à mesure de leur ajout.

Interpréteur et Vérificateur Exécutables Vérifiés : En plus de la spécification mécanisée, un interpréteur et un vérificateur de type exécutables ont été définis et vérifiés formellement. Ces artefacts peuvent être validés expérimentalement en utilisant le conformance test suite officiel de WebAssembly.

Ces avancées dans la spécification mécanisée ont contribué à l'amélioration de la spécification officielle en identifiant des problèmes et en influençant son développement. Cependant, bien que la spécification mécanisée renforce la confiance dans la cohérence du langage, elle ne supprime pas la nécessité de tests, comme en témoigne l'utilisation de la suite de tests de conformité de WebAssembly et la réalisation d'expériences de fuzzing pour valider la spécification mécanisée.
