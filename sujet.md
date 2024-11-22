# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1.	
	Source : 
	- https://www.lemonde.fr/pixels/article/2024/10/08/microsoft-word-decouverte-d-un-bug-qui-supprime-un-document-au-lieu-de-le-sauvegarder_6346684_4408996.html
	- https://support.microsoft.com/fr-fr/office/locally-saved-word-files-with-capitalized-file-extensions-or-in-the-title-may-be-deleted-after-save-5e28f8c2-32d0-487b-b237-9c7c74d25f84
	
	Description du bug:
		Ce bug est apparu dans Microsoft Word et ne touchait que les fichiers enregistrer en local et pas ceux utilisant les sauvegardes en lignes. Elle a été résolu le 8/10/24.
		Le bug apparaissait lorsque l'on enregistrait notre fichier Word à partir de l'invite, qui s'ouvre lorsque l'on ferme le document sans avoir enregistrer les modifications. Celui-ci pouvait, sous certaines conditions, supprimer notre document au lieu de l'enregistrer en local. Pour ce faire, le fichier devait avoir un nom d'utilisant une certaine syntax. C'est à dire, le nom du document devait se terminé par une extention écrite en majuscule, tel que .DOCS ou .RTF ou alors si cette extension contient un #.   
		
	Type: Globale
	
	Répercussion:
		De nombreux utilisateur, proféssionnel ou particulier, ont pu voir leur travaux disparaître sans y préter attention. Pour des documents sans valeur particulière c'est embéttant mais pour d'autres types de documents tel que des documents administratif de facturation ou de comptabilité, le bug est relativement problèmatique.
	Avis:
		Ce bug aurait pu être détecter à partir d'un scénario spécifique puisqu'il n'apparait que sous l'utilisation de l'invite de commande de sauvegarde et à partir d'une certaines syntax. Néanmoins, ce scénario est tellement spécifique qu'il est logique de passé outre et de ne pas le considérer.
	
	
2.	Apache Commons
https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-850?filter=doneissues&orderby=updated+ASC

	Collections-850: testMultiValuedMapIterator is non-deterministic

	Type: Local
		
	Problème:
		Le bug presenter est un bug potentiel qui pouvait apparaître et qui a été mis en évidence par le biais de l'outil nondex.
		Celui-ci a détécter un potentiel problème dans le test d'une classe (multimap). En effet, l'implémentation de ce test, sugére que l'ordre du map observer est toujours le même alors que l'utilisation de l'itérator et du map ne permet pas d'affirmer que l'ordre est maintenue.

	Solution:
		On peut soit considerer que le test d'origine implemente la volonté orignal de la classe et donc modifier la classe pour resoudre le problème neanmoins on affecterait l'ensembles des utilisateurs utilisant cette classe.
		Pour ne pas affecter les autres utilisateurs, il a été décider de modifier le test et de passé outre l'ordre.
	
3.	Netflix Chaos

	- quelles sont les expériences concrètes qu'ils effectuent ?
	
		- Chaos Monkey : Interruption aléatoire d'instances de machines virtuelles pour tester la résilience des microservices.
		- Chaos Kong : Simulation de pannes complètes d’une région.
		- Failure Injection Testing (FIT) : Injection de défaillances, telles que des requêtes échouées ou des retards de communication.
		- Tests de surcharge : Émulation de pics de trafic.
	
	- quelles sont les exigences pour ces expériences ?
	
		- Une infrastructure cloud distribuée.
		- Une capacité à surveiller en temps réel les métriques critiques du système.
		- Des environnements de test en production.
		
	- quelles sont les variables qu'ils observent et quels sont les principaux résultats qu'ils ont obtenus ?
		Le nombre de démarrages de flux par seconde (SPS), qui indique l'état général du système. La latence des requêtes, l'utilisation du processeur, et les erreurs sur des services spécifiques pour évaluer les impacts locaux​.
		
		À la fin d'un test, ils peuvent conclure 2 chose, soit ils peuvent avoir confiance dans la capacité du système à maintenir le comportement en présence des variables, soit une faiblesse a été mise en évidence.

4.	WebAssembly
	
	expliquez quels sont les principaux avantages d'avoir une spécification formelle pour WebAssembly. 
		- Portabilité et Cohérence: Une spécification garantit que WebAssembly se comporte de manière identique sur toutes les plateformes.
		- Sécurité Renforcée: il minimise les risques de comportements imprévus ou de vulnérabilités.
		- Facilité de Vérification et de Validation: Les développeurs peuvent utiliser des outils de preuve pour garantir que leur implémentation respecte les attentes définies dans la spécification.

	
		
	cela signifie-t-il que les implémentations WebAssembly ne doivent pas être testées ?
		L'utilisation de WebAssembly ne remplace pas les tests. Ils est toujours possible de voir appaître des implementations eroné qui sont source d'erreur. L'environnement web est aussi en perpetuel evolution et demande à être en permanence surveiller pour verifier que notre code fonctionne toujours. 
		Sa potentiel robustesse et sa fiabilité, ne permet donc pas de passer outre les tests, qui restent indispensables pour garantir la robustesse dans des scénarios réels.
		
5.	WebAssembly Bis

	Selon l'auteur de ce deuxième article, quels sont les principaux avantages de la spécification mécanisée ? 
		- Précision et Fiabilité
	A-t-elle contribué à améliorer la spécification formelle originale du langage ? 
		Oui ses travaux ont permis d'améliorer la spécification formelle.
	Quels autres artefacts ont été dérivés de cette spécification mécanisée ? 
		Un interpréteur, permettant d'exécuter des programmes WebAssembly avec des garanties formelles sur leur comportement. Et un vérificateur, prouvant la conformité des programmes avec le système de types de WebAssembly. Ces artefacts sont intégrés avec le code de référence officiel pour tester leur compatibilité et fiabilité​.
		
	Comment l'auteur a-t-il vérifié la spécification ? 
		La spécification mécanisée a été testée en utilisant la suite de tests officielle de conformité de WebAssembly et des tests différentiels contre plusieurs moteurs commerciaux, garantissant sa robustesse et sa précision
	Cette nouvelle spécification supprime-t-elle le besoin de tests ?
		Non, des erreurs peuvent toujours apparître, tel que des erreurs humaines.
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
		
