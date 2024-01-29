# Practical Session #1: Introduction - Ilan HUCHÉ - Aiman AOUAD

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

# Answers - Ilan HUCHÉ - Aiman AOUAD

Le bug de l'an 2000, également connu sous le nom de Y2K, était une erreur de programmation qui affectait les systèmes informatiques utilisant des dates. Cette erreur était due au fait que les dates étaient souvent stockées en utilisant seulement deux chiffres pour l'année, ce qui pouvait entraîner des confusions entre les années 1900 et 2000.

## Type de bug :

Le bug de l'an 2000 était un bug global, car il affectait tous les logiciels utilisant des dates. Il s'agissait d'une erreur de conception systémique, et non d'une erreur de programmation dans un seul logiciel.

## Panne :

Le bug de l'an 2000 aurait pu entraîner une panne généralisée des systèmes informatiques, y compris des systèmes critiques tels que les systèmes bancaires, les systèmes de distribution d'électricité et les systèmes de contrôle du trafic aérien.

## Répercussions :

Les répercussions du bug de l'an 2000 auraient été catastrophiques pour les clients/consommateurs et l'entreprise ou l'entité à l'origine du programme défectueux. Les clients/consommateurs auraient pu perdre des données importantes, tels que des comptes bancaires, des informations de santé ou des réservations de voyage. L'entreprise ou l'entité à l'origine du programme défectueux aurait pu subir des dommages importants à sa réputation et à ses finances.

## Test de scénario :

Oui, tester le bon scénario aurait pu permettre de découvrir le bug de l'an 2000. Les tests auraient dû inclure des scénarios qui vérifiaient la façon dont le logiciel traitait les dates. Par exemple, les tests auraient dû tester le logiciel avec des dates proches de l'an 2000, pour voir si le logiciel comportait des erreurs.

Dans le cas du bug de l'an 2000, les tests auraient dû être effectués sur un large éventail de logiciels, pour s'assurer que le bug était présent dans tous les logiciels utilisant des dates.

Il est important de noter que les tests ne peuvent pas garantir que tous les bugs seront détectés. Cependant, ils peuvent aider à réduire le risque de bugs en identifiant les problèmes potentiels.

Dans le cas du bug de l'an 2000, les tests auraient pu aider à détecter le bug bien avant 2000. Cela aurait permis aux développeurs de logiciels de corriger le bug à temps pour éviter les problèmes potentiels.

## Conclusion :

Le bug de l'an 2000 est un exemple de la façon dont une erreur de programmation peut avoir des conséquences catastrophiques. Ce bug a été découvert à temps, mais il est important de se rappeler que les tests de logiciels sont essentiels pour réduire le risque de bugs.


## Bug Local Résolu dans le Projet Apache Commons Collections :

Au sein du projet Apache Commons Collections, un bug local a été identifié et résolu sous le numéro COLLECTIONS-794. Ce problème spécifique était lié au test AbstractCountingBloomFilterTest, qui erronément supposait que le filtre Bloom utilisait un entier pour suivre les compteurs. Cette supposition limitait la portabilité du filtre Bloom, car toute implémentation utilisant une taille de stockage plus petite que celle présumée dans le test échouait systématiquement.

Description du Bug et Solution Apportée :
Le bug était inhérent au test du filtre Bloom, qui était basé sur une hypothèse incorrecte quant au type de données utilisé pour suivre les compteurs. Cette mauvaise supposition restreignait l'applicabilité du filtre Bloom, car toute implémentation avec une taille de stockage inférieure à celle présumée dans le test ne réussissait pas les vérifications. La solution apportée a consisté à rectifier cette hypothèse erronée dans le test, permettant ainsi au filtre Bloom d'employer des tailles de stockage plus petites tout en réussissant le test.

Tests Additionnels pour la Détection Future :
Dans le processus de résolution, les contributeurs du projet Apache Commons Collections ont pris la précaution d'introduire de nouveaux tests afin de garantir que le bug serait détecté s'il devait réapparaître ultérieurement. Ces tests supplémentaires ont été élaborés pour assurer la compatibilité continue de l'implémentation du filtre Bloom avec des tailles de stockage variables, renforçant ainsi la résilience du code. En résumé, la correction immédiate du bug a été complétée par des mesures préventives, améliorant la robustesse du projet dans son ensemble.


## Les experiances concretent que Netflix réalise ET éxisanges :

	Expériences avec Chaos Monkey :

		Terminaison aléatoire d'instances de machines virtuelles.
		Exigences : Encourager la conception de services résilients.
		Variables Observées : Réaction des services aux défaillances d'instances.
		Résultats : Succès dans la conception de services capables de résister aux défaillances d'instances.
			
	Chaos Kong Exercises :

		Type : Simulation de la défaillance d'une région entière Amazon EC2.
		Exigences : Tester la résilience à des défaillances plus larges.
		Variables Observées : Réaction du système à la défaillance d'une région.
		Résultats : Non spécifiés dans le texte, mais l'objectif est d'assurer la continuité malgré des pannes étendues.
		
	Failure Injection Testing (FIT) :

		Type : Injection intentionnelle d'erreurs dans les requêtes entre les services Netflix.
		Exigences : Vérifier la dégradation gracieuse du système lors d'erreurs simulées.
		Variables Observées : Réaction du système aux erreurs injectées.
		Résultats : Évaluation de la capacité du système à maintenir la fonctionnalité malgré des erreurs simulées.

-L'article mentionne que des entreprises comme Amazon, Google, Microsoft et Facebook appliquent des techniques similaires de test de résilience (Chaos Engineering).

- Des expériences similaires à celles menées par Netflix en Chaos Engineering peuvent être adaptées par d'autres organisations en fonction de leur infrastructure spécifique. Les variables à observer varieraient en fonction des composants critiques du système, tels que la réaction aux défaillances matérielles ou aux erreurs de configuration. L'adaptation aux contextes spécifiques impliquerait des variables contextuelles dépendant des fonctionnalités cruciales pour chaque organisation; par exemple, des entreprises financières pourraient simuler des erreurs dans des transactions, tandis qu'une entreprise de commerce électronique pourrait simuler des pannes de serveurs de paiement. L'encouragement à automatiser ces expériences vise à assurer une répétabilité constante malgré l'évolution du système, et la fréquence d'automatisation serait ajustée en fonction des besoins particuliers de chaque organisation.

## Avantages de la Spécification Formelle pour WebAssembly :

La spécification formelle de WebAssembly offre plusieurs avantages cruciaux. Tout d'abord, elle établit une base claire et précise pour le comportement attendu du langage, fournissant ainsi une référence commune pour les développeurs et les implémenteurs. Cette clarté contribue à l'interopérabilité entre différentes implémentations, permettant aux développeurs de créer du code WebAssembly avec l'assurance qu'il se comportera de manière cohérente sur divers navigateurs et plates-formes. De plus, la spécification formelle facilite la vérification formelle, une approche rigoureuse pour garantir la correction du langage dans toutes les situations. En réduisant l'ambiguïté et en offrant une compréhension précise du langage, la spécification formelle renforce la confiance dans la fiabilité et la stabilité de WebAssembly.

Rôle Complémentaire des Tests :
Malgré les avantages de la spécification formelle, cela ne signifie pas que les implémentations de WebAssembly ne doivent pas être testées. Les tests demeurent essentiels pour garantir la conformité pratique des implémentations et pour détecter d'éventuelles erreurs ou nuances non anticipées. La spécification formelle fournit une base solide, mais les tests offrent une validation dans des environnements réels et divers. Ils permettent de s'assurer que les implémentations fonctionnent correctement, sont performantes et répondent aux besoins variés des développeurs. En somme, les tests et la spécification formelle jouent des rôles complémentaires, contribuant ensemble à garantir la qualité, la cohérence et la robustesse de l'écosystème WebAssembly.



## Avantages de la Spécification Mécanisée de WebAssembly :

L'article proposant une spécification mécanisée de WebAssembly utilisant Isabelle met en avant plusieurs avantages significatifs. Tout d'abord, la mécanisation offre une rigueur et une précision supplémentaires à la spécification, permettant une vérification formelle plus approfondie. La spécification mécanisée facilite également l'automatisation des processus de vérification, contribuant ainsi à la fiabilité du langage. De plus, l'auteur souligne que la spécification mécanisée améliore la clarté et la compréhension de la spécification formelle originale, offrant ainsi une base plus solide pour les développeurs et les chercheurs.

Amélioration de la Spécification Formelle et Artefacts Dérivés :
La spécification mécanisée joue un rôle crucial dans l'amélioration de la spécification formelle originale de WebAssembly. Elle offre une vérification plus approfondie et identifie des aspects précis où la spécification peut être renforcée. De plus, cette mécanisation conduit à la dérivation d'artefacts supplémentaires tels que des preuves formelles de propriétés importantes du langage. Ces preuves renforcent la confiance dans la spécification et ses garanties.

Vérification de la Spécification et Rôle Complémentaire des Tests :
L'auteur a utilisé l'outil Isabelle pour vérifier la spécification mécanisée de WebAssembly, démontrant ainsi la cohérence et la correction de la spécification à travers une approche formelle. Cependant, même avec une spécification mécanisée, les tests demeurent essentiels pour valider la conformité pratique et la performance des implémentations de WebAssembly dans des environnements réels. La spécification mécanisée renforce la vérifiabilité, mais les tests restent nécessaires pour garantir la robustesse du langage dans des cas d'utilisation variés. En fin de compte, les tests et la spécification mécanisée se complètent mutuellement pour assurer la qualité et la fiabilité de WebAssembly.

		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
