# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

## Question 1

### Bug : Mise à jour crowdStrike
- [article 1](https://www.usine-digitale.fr/article/crowdstrike-pourquoi-la-mise-a-jour-d-un-antivirus-a-paralyse-les-entreprises-du-monde-entier.N2216441)
- [article 2](https://trustmyscience.com/crowdstrike-revele-cause-panne-mondiale-microsoft-erreur-etudiants-programmation-apprennent-eviter/)

### Despcription du bug : 
Le 19 juillet 2024, une mise à jour du logiciel de cybersécurité Falcon sensor de CrowdStrike à causé une panne mondiale. 
Falcon sensor est un EDR (Endpoint Detection and Response) concu pour détecter et prévenir les menaces en temps réel. Il fonctionne en surveillant les activités au niveau du noyau du système d’exploitation Windows.
En ayant un accès privilégié au cœur du système, le capteur peut analyser les comportements et les événements système pour identifier rapidement les anomalies. Le problème était que la mise à jour du logiciel Falcon sensor comportait un champ d'entrée supplémentaire non détecté et donc celà provoquait une incohérence de comptage qui conduit à un accès mémoire hors limite provoquant l'effondrement des systèmes Windows (BSOD).

Ce bug a affecté les systèmes Windows, entraînant des redémarrages en boucle et des BSOD sur de nombreux appareils. Des secteurs comme les aéroports, les hôpitaux, et les banques ont subi de lourdes perturbations comme des annulations de vols.

Les conséquences pour les consommateurs incluent des désagréments pratiques et des pertes financières. Pour CrowdStrike, cet incident a affecté sa réputation.

Tester le scénario de mise à jour sur un environnement varié, incluant les versions et configurations les plus utilisées, aurait pu identifier ce bug.

Il s'agit donc là d'un bug global.

## Question 2

### Bug : BloomFilter : supprimer mergeInPlace
- [lien](https://issues.apache.org/jira/browse/COLLECTIONS-829)
- [enfant de](https://issues.apache.org/jira/browse/COLLECTIONS-728)

Dans le projet Apache Commons, le BloomFilter est une implémentation de la structure de données probabiliste du même nom. Un Bloom Filter est utilisé pour tester si un élément est membre d’un ensemble, avec une possibilité de faux positifs (mais pas de faux négatifs).

Ce bug est global car il affecte le comportement de base et la fiabilité de la fusion des objets BloomFilter,
ce qui peut amener à des erreurs à l'échelle du système si la méthode est mal utilisé

### Description du bug : 
La méthode mergeInPlace dans la classe BloomFilter posait des problèmes de fiabilité.
Elle permettait une fusion directe, mais son résultat pouvait être invalide (ex. dans ArrayCountingBloomFilter)
et son retour était souvent ignoré. Cela entraînait des comportements imprévisibles.

### Conséquences possibles :
Incohérence des données : Si une fusion échoue silencieusement, la copie du filtre pourrait donner des résultats erronés lors de l’interrogation (par exemple, signaler la présence d’éléments qui n’ont jamais été ajoutés).

Scénario : Les utilisateurs pourraient supposer que la fusion a réussi, ce qui introduit des bugs dans des systèmes utilisant le filtre.

Difficulté de débogage : L’absence de signal clair pour l’échec rend difficile l’identification et la résolution du problème.

### Solution : 
La méthode mergeInPlace a été supprimée et remplacée par une approche plus claire et sécurisée :

- Désormais, pour effectuer une fusion, l’utilisateur doit créer une copie du filtre existant et fusionner dans cette copie.
- Nouveau modèle recommandé :
```java
BloomFilter bf = bf1.copy();  // Crée une copie du filtre bf1.
bf.merge(x);                  // Fusionne un autre filtre dans la copie.
```
Cela garantit que le filtre original reste intact et que le comportement est explicite pour les utilisateurs.

### Les contributeurs du projet ont-ils ajouté de nouveaux tests pour s'assurer que le bug est détecté s'il réapparaît à l'avenir ?
Pas d'informations supplémentaires trouvées pour savoir si des nouveaux tests ont été rajouté par la suite (difficile de trouver la correspondance des dates). 
[lien des tests en rapport](https://github.com/apache/commons-collections/blob/master/src/test/java/org/apache/commons/collections4/bloomfilter/AbstractBloomFilterTest.java)


## Question 3

### Quelles sont les expériences concrètes qu'ils effectuent ?
Les expériences de Chaos Engineering effectuées par Netflix :

- **Chaos Monkey** : Termination aléatoire d'instances de machines virtuelles pour tester la robustesse des services individuels.
- **Chaos Kong** : Simulation de la panne d'une région entière d'Amazon EC2.
- **Failure Injection Testing (FIT)** : Introduction d'échecs dans les requêtes entre services pour vérifier si le système se dégrade de manière contrôlée.
- **Tests de surcharge et de latence** : Injection de latences ou surcharge des services pour observer les réponses.

Ces expériences se concentrent sur des scénarios de panne réalistes, comme la défaillance de matériel ou des erreurs réseau.

### Quelles sont les exigences pour ces expériences ?

Les exigences sont :

- Une définition claire de l'état stable, mesurée par des métriques comme les "Stream Starts per Second (SPS)".
- Hypothèses sur le comportement du système lors de perturbations.
- L'exécution d'expériences directement en production pour garantir un environnement réaliste.
- Automatisation pour répliquer régulièrement les tests dans un système en constante évolution.

### Quelles sont les variables qu'ils observent et quels sont les principaux résultats qu'ils ont obtenus ?

- Variables observés :
  - Métriques de haut niveau, comme les SPS.
  - Métriques fines, comme la latence des requêtes ou l'utilisation du CPU, utilisées pour diagnostiquer des anomalies à un niveau micro.
- Résultats principaux :
  - Amélioration de la résilience des services à des pannes de composants individuels ou régionaux.
  - Identification proactive des faiblesses du système.
  - Renforcement des processus de dégradation contrôlée, où une panne d'un service non critique n'affecte pas l'expérience utilisateur globale.

### Netflix est-elle la seule entreprise à effectuer ces expériences ?
Non, d'autres grandes entreprises comme Amazon, Google, Microsoft, et Facebook utilisent des techniques similaires pour tester la résilience de leurs systèmes. Par exemple :

- **Facebook** : A éteint un centre de données entier pour évaluer la résilience.
- **Microsoft Azure** : A intégré des pratiques de Chaos Engineering dans Azure Search.
- **Amazon** : Implémente des tests similaires pour renforcer ses systèmes distribués.

### Spéculez comment ces expériences pourraient être menées dans d'autres organisations.

Dans d'autres organisations, les expériences pourraient inclure :

- **Tests d'intégrité pour l'e-commerce** : Simuler des pannes dans des systèmes critiques comme la gestion des stocks ou les passerelles de paiement, en observant les transactions réussies par seconde.
- **Industries financières** : Simuler des pertes de connexion ou des erreurs dans les systèmes de trading en temps réel pour évaluer la tolérance aux fautes et la récupération.
- **Santé** : Tester les réponses à des perturbations dans la transmission de données médicales critiques.

Variables potentielles à observer :

- Taux de disponibilité.
- Temps de récupération après panne.
- Effets sur la latence.

## Question 4 

### Avantages de la spécification formelle pour WebAssembly

#### 1. **Précision et clarté**
- Une spécification formelle décrit les fonctionnalités et le comportement attendu de WebAssembly avec une précision mathématique. Cela élimine toute ambiguïté dans l’interprétation de la norme.

#### 2. **Interopérabilité et uniformité**
- Les implémentations WebAssembly par différents moteurs (comme V8, SpiderMonkey) suivent la même norme, garantissant une cohérence dans le comportement à travers les plateformes.

#### 3. **Validation automatique**
- Grâce à la spécification formelle, les outils automatiques peuvent vérifier la conformité des implémentations, détecter des erreurs et valider les programmes WebAssembly de manière fiable.

#### 4. **Base pour les analyses et optimisations**
- La spécification permet de concevoir des analyses statiques et dynamiques robustes, utiles pour les optimisations et la vérification de la sécurité.

#### 5. **Sécurité renforcée**
- En s’appuyant sur la spécification, il est possible de prouver des propriétés comme la sécurité mémoire ce qui est crucial pour exécuter du code provenant de sources non fiables.


### Test des implémentations WebAssembly

Malgré la spécification formelle, les implémentations WebAssembly doivent être testées pour plusieurs raisons :

#### 1. **Erreurs dans l’implémentation**
- La spécification formelle peut être correcte, mais les implémentations pourraient contenir des bogues ou des erreurs de traduction.

#### 2. **Différences d’environnement**
- Les moteurs WebAssembly sont aussi intégrés dans des environnements complexes (comme les navigateurs). Les interactions entre le moteur et l'environnement doivent être testées.

#### 3. **Performance**
- Les tests de performance sont nécessaires pour s'assurer que les implémentations répondent aux attentes en termes de vitesse et d'efficacité.

#### 4. **Conformité aux extensions futures**
- Avec des extensions ou modifications de WebAssembly, les tests garantissent que de nouvelles fonctionnalités sont correctement intégrées sans introduire de régressions.

### Conclusion
En résumé, la spécification formelle est une base essentielle pour concevoir et valider WebAssembly, mais elle ne remplace pas les tests des implémentations.

## Question 5

### 1. Principaux avantages de la spécification mécanisée

- La spécification mécanisée a permis de découvrir et corriger plusieurs erreurs dans la spécification officielle de WebAssembly, notamment des failles dans le système de types qui le rendaient initialement non sûr.
- Elle garantit des preuves formelles de propriétés comme la cohérence et la progression du système de types, ce qui n'était pas assuré par la spécification manuscrite.
- Elle fournit des artefacts vérifiés, comme :
  - Un **interpréteur exécutable**.
  - Un **vérificateur de types**, facilitant la validation et les tests.


### 2. Contribution à l'amélioration de la spécification formelle originale du langage

La spécification mécanisée a contribuer à améliorer la version originale :
- Elle a révélé plusieurs défauts dans la spécification officielle, comme :
  - La propagation incorrecte de l'instruction `Trap`.
  - Une définition erronée de l'instruction `Return`, permettant des cas invalides.
- Des corrections ont été intégrées dans la spécification officielle.


### 3. Autres artefacts dérivés de cette spécification mécanisée

Plusieurs artefacts ont été dérivés de la spécification mécanisée :
- Un **interpréteur exécutable vérifié**.
- Un **vérificateur de types prouvé correct**.
- Ces artefacts permettent :
  - D’exécuter des programmes conformes à la spécification.
  - De valider expérimentalement le modèle via des tests contre des implémentations industrielles.


### 4. Vérification de la spécification et suppression du besoin de tests

#### Vérification de la spécification
- L'auteur a prouvé deux propriétés fondamentales du système de types :
  - **Préservation** : les types sont maintenus à chaque étape d'exécution.
  - **Progression** : un programme bien typé avance ou signale une exception.
- Ces preuves ont nécessité des lemmes auxiliaires complexes pour les cas impliquant :
  - Les contextes récursifs.
  - Les flux de contrôle structurés (`br`, `Return`).

#### La nécessité des tests
- Même si la vérification renforce la confiance dans le modèle, elle ne supprime pas le besoin de tests pratique.
- Des erreurs peuvent toujours se produire dans :
  - Les **interfaces non vérifiées**.
  - Les **implémentations externes** non couvertes par la spécification.
