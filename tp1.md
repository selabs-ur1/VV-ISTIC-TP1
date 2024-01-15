## Réponses

1. Erreur du logiciel, créé par Raytheon, de système antimissile MIM-104 Patriot empêchant la détection d'un missile en approche. Cette erreur est à porté globale car découverte après l'échec de l'interception d'un missile causant la mort de 28 soldats américains en Arabie Saoudite. Elle est dû à l'imprécision de l'horloge du système resté en fonctionnement près de 100 heures causant un décalage d'1/3 de seconde résultant en un désynchronisation du système de suivi. Des tests approfondis aurait pu prévenir l'apparition de décalage de l'horloge.

2. https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-766?filter=doneissues Erreur dans un fichier de test à porté global car empêchant le build d'intégration continu sur JDK 11+. Elle est dû à l'ajout de la méthode Math.floorMod(long,int) en JDK 11+ alors que la méthode Math.floorMod(long,long) existait déjà sous JDK 8, qui est donc appelé à la place. L'erreur à été contournée an ajoutant un cast int vers long. Il n'y a pas eu de test suplémentaire.

3. 
Les expériences sont directement réalisées en production et sont composées de plusieurs scénarios.
Scénario 1 : Terminer des instances de machines virtuelles,
Scénario 2 : Injecter de la latence dans les requêtes entre les services,
Scénario 3 : Échec des requêtes entre les services,
Scénario 4 : Échec d'un service interne,
Scénario 5 : Rendre une région entière d'Amazon indisponible.
Exigence : Variabilité des stimuli, historique des pannes, simulation d'évènements presque impossible (e.g. : mise hors ligne d'une région entière).
Variable observée : Réponses du système, impact sur les utilisateurs.
Principaux résultats obtenus : Identification des défaillances potentielles, évaluation de la résilience.
L'ingénierie du chaos est utilisée dans de nombreuses autres entreprises, de manière similaire à Netflix, en introduisant des problèmes en production ou si possible en test pour évaluer par exemple la gestion des différents systèmes face à des surcharges, des coupures de services ou des erreurs dans des flux de données en surveillant la réaction des systèmes et les impacts.

4. 
The main advantages of having a formal specification for WebAssembly are :
- Clear Semantics :  It provides a precise and unambiguous definition of the semantics of WebAssembly
- Interoperability : Interested producers can define common ABIs on top ofWebAssembly
- Safe execution : It aids in the validation of memory accesses and other potential security risks.
- Guidance for updates : It provides guidance on how new features should be integrated,
ensuring backward compatibility where necessary.
- Formal verification : It can be used as a basis for formal verification.
In your opinion, does this mean that WebAssembly implementations should not be tested ?
Testing is still a necessary approach to ensure the reliability, and security of WebAssembly.

5. 
Principaux avantages :
- Eyeball Closeness : Correspondance ligne par ligne de la spécification mécanisée avec la spécification officielle,
- Formalisme Strict : La spécification mécanisée respecte les idéaux d'une spécification formelle, et les règles de réduction et de typage de WebAssembly sont définies en notation formelle.

Améliorations apportées à la spécification originale :
- Correction d'Erreurs : Des erreurs significatives dans la spécification originale ont été identifiées et corrigées (e.g. : problèmes liés à la propagation des exceptions résolus).

Autres Artefacts dérivés de la Spécification Mécanisée :
- Executable Type Checker : Un vérificateur de type exécutable a été défini séparément de la spécification mécanisée et a été prouvé comme étant correct par rapport à la relation de typage.

Vérification de la Spécification :
- Preuve de Soundness : Lemmes auxiliaires, dont des cas de contextes d'exécution récursifs avec des structures de contrôle,
- Validation et Fuzzing : Un interpréteur exécutable, combiné avec un parseur et un lien de référence, a réussi tous les tests de conformité du langage. Des tests différentiels ont été effectués avec d'autres moteurs WebAssembly sans trouver d'erreurs dans l'implémentation.

Nécessité de Tests :
La spécification mécanisée n'élimine pas le besoin de tests. Les tests restent utiles pour détecter des problèmes qui pourraient ne pas être couverts par la spécification formelle.