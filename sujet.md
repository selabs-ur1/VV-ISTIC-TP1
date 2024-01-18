# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### 1. bug choisi : CVE-2023-49920 - Apache Airflow

La vulnérabilité dans Apache Airflow 2.7.0 à 2.7.3 permet à un attaquant de déclencher l'exécution d'un 'Directed Acyclic Graph' (DAG) via une requête GET sans validation CSRF. Les DAGs dans Airflow sont des graphiques acycliques dirigés représentant des tâches organisées dans un ordre spécifique. Dans le contexte de la vulnérabilité, les DAGs sont configurés pour être déclenchés via une requête GET. Une validation 'Cross-Site Request Forgery' (CSRF) quant à elle, est une mesure de sécurité utilisée pour prévenir les attaques où un attaquant tente de manipuler les actions d'un utilisateur authentifié. Elle implique l'utilisation de jetons CSRF uniques associés à la session de l'utilisateur, vérifiant ainsi l'origine légitime des requêtes HTTP. La vulnérabilité réside dans le fait qu'aucune validation CSRF n'est effectuée lorsqu'un DAG est déclenché via une requête GET. Cela signifie qu'un attaquant pourrait exploiter cette faille en incitant un utilisateur qui a l'interface utilisateur d'Airflow ouverte à déclencher l'exécution de DAGs sans le consentement de cet utilisateur. En d'autres termes, un site malveillant ou une requête manipulée pourrait déclencher des tâches de manière non autorisée, compromettant la sécurité et la confidentialité des opérations planifiées dans Apache Airflow.

La portée de ce bug est globale, affectant les versions 2.7.0 à 2.7.3 d'Apache Airflow.

Les clients et consommateurs de ces versions d'Apache Airflow sont exposés à des exécutions non autorisées de DAGs. Cela peut compromettre des opérations planifiées, en particulier si des DAGs sensibles sont déclenchés sans le consentement de l'utilisateur.

Apache pourrait subir des coûts associés à la résolution de cette vulnérabilité, tels que le développement et le déploiement de correctifs.

Afin d'éviter cela, des tests de sécurité plus approfondis, en particulier des scénarios impliquant des requêtes GET, des validations CSRF ou le déclenchement de DAGs, auraient possiblement identifié et atténué cette vulnérabilité.


### 2. bug choisi : COLLECTIONS-813 : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-813?filter=doneissues

Le bug est local et spécifique à une méthode (CollectionUtils.retainAll) de la bibliothèque Apache Commons Collections.

La méthode CollectionUtils.retainAll(Iterable<E> collection, Iterable<? extends E> retain, Equator<? super E> equator) ne lance pas de NullPointerException (NPE) comme prévu lorsque l'un de ses paramètres, dans ce contexte 'Equator<? super E> equator', est nul. La documentation indique qu'une NPE devrait être lancée si l'un des paramètres est nul et dans le cas de test fourni, où le paramètre 'Equator ..' est intentionnellement défini sur null, ne génère pas de NPE comme attendu.

Le bug a été corrigé dans la version 4.5 de la bibliothèque Apache Commons Collections. La correction consiste en une vérification sur le paramètre 'Equator<? super E> equator' à l'aide de Objects.requireNonNull. Cette vérification garantit qu'une NullPointerException sera lancée si le paramètre 'Equator ..' est nul.

Le contributeur mentionne que le cas de test fourni, qui échouait avant la correction, réussit maintenant dans la branche principale actuelle (4.5-SNAPSHOT). Il n'est pas mentionné dans les informations fournies si de nouveaux tests ont été ajoutés.


### 3. Netflix et l'ingénierie du chaos

Netflix mène des expériences d'ingénierie du chaos pour améliorer la fiabilité de ses systèmes distribués. Parmi les expériences on peut citer, Chaos Monkey, Chaos Kong ou Failure Injection Testing (FIT). Chaos Monkey termine de manière aléatoire les instances de machines virtuelles pendant des heures de travail normales. Chaos Kong quant à lui, simule la défaillance de toute une région comme par exemple 'Amazon Elastic Compute Cloud' (EC2, service cloud de la branche AWS de Amazon). Pour finir, FIT provoque intentionnellement l'échec des requêtes entre les services de Netflix.

Ces expériences impliquent des hypothèses, des variables indépendantes (par exemple une panne de serveur), des variables dépendantes (par exemple les sorties mesurables du système) et un contexte. L'objectif est d'automatiser les expériences, afin de refléter la nature évolutive du système.

La variable principale est la métrique '(Stream) Starts Per Second' (SPS), représentant la santé globale du système. Les deux autres métriques utilisées sont, la charge CPU et la latence des requêtes, pour vérifier les impacts possibles au niveau du service.

Les résultats visent à améliorer la capacité du système à maintenir son comportement aussi stable que possible, dans différentes conditions. Par exemple, Chaos Monkey a conduit les ingénieurs à concevoir des services capables de résister aux défaillances d'instances individuelles.

Netflix note que des organisations telles qu'Amazon, Google, Microsoft ou encore Facebook, appliquent des techniques similaires pour tester la résilience de leurs systèmes.

Dans d'autres organisations, les expériences pourraient impliquer la simulation de divers événements du monde réel, comme des pannes matérielles ou des pics inattendus de demandes client. Les variables à observer pourraient être, les achats terminés par seconde pour un site de e-commerce ou le nombre d'annonces vues par seconde pour un service de diffusion d'annonces.


### 4. WebAssembly

La spécification formelle de WebAssembly offre une stabilité et une cohérence pour l'interopérabilité, elle établie une base fiable pour le développement d'outils et garantie une interprétation uniforme du code sur divers navigateurs. En définissant clairement la syntaxe, le comportement, et les structures de contrôle, elle facilite la détection précoce d'incohérences, contribuant à l'établissement d'une norme robuste et évolutive. Cette formalisation renforce la confiance dans la fiabilité des implémentations, réduisant les risques d'interprétations divergentes et assurant une expérience utilisateur stable sur différentes plates-formes.

Néanmoins, cela ne signifie pas que les tests d'implémentation sont inutiles. Ces tests servent à valider le comportement réel des situations pratiques, identifier les bugs spécifiques et assurer la robustesse face aux différences entre les implémentations.


### 5. La Spécification Mécanisée

La spécification mécanisée offre plusieurs avantages. Tout d'abord, elle fournit une représentation formelle et précise du langage, facilitant ainsi un raisonnement rigoureux et des preuves concernant ses propriétés. De plus, elle maintient une correspondance étroite avec la spécification officielle, assurant une fidélité ligne par ligne. La mécanisation permet également l'identification et la correction d'erreurs dans la spécification officielle, contribuant ainsi à son amélioration globale.

La spécification mécanisée a eu un impact significatif sur la spécification formelle originale du WebAssembly. En identifiant et corrigeant des erreurs dans la spécification officielle, elle a contribué à son amélioration. De plus, la spécification mécanisée sert de référence pour la preuve de la cohérence du système de types, renforçant ainsi la robustesse de la spécification originale.

Plusieurs artefacts ont dérivés de la spécification mécanisée. Un vérificateur de types exécutable a été créé, offrant un outil efficace et prouvé pour vérifier les types des programmes WebAssembly. De même, un interpréteur exécutable a été mis en œuvre, fournissant un outil vérifié et pratique pour l'exécution du code WebAssembly.

La spécification est vérifiée à l'aide d'Isabelle, un assistant de preuve formelle. Des relations inductives correspondant aux règles de réduction et de typage du WebAssembly sont définies. Des fonctions exécutables séparées pour la vérification des types et l'interprétation sont définies et prouvées comme correctes par rapport à leurs relations respectives.

Bien que la spécification mécanisée renforce la fiabilité de l'implémentation WebAssembly, les tests restent necessaires. Ils garantissent que l'implémentation est conforme à la spécification et peut gérer divers scénarios du monde réel. La vérification mécanisée et les tests peuvent se compléter mutuellement pour assurer une assurance de correction plus robuste.


