# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. 

Le bug "heartbleed" est un bug de programmation (un défaut de conception) local (car la spécification est bien faite c'est l'implémentation qui est mal faite) qui fait cracher à un serveur des données qu'il n'est pas censé communiquer (login, mot de passe, données banquaires, clés de cryptage) à cause de la réponse à la requête "heartbeat" qui cherche à s'assurer que le contact avec le serveur est toujours actif. Le bug a été trouvé par "Codenomicon" en augmentant le "SafeGuard" dans leurs outils de test de sécurité.
Pour les clients les répercussions sont d'avoir potentiellement perdus des données très sensibles, comme les serveurs qui peuvent avoir perdus des documents critiques pour des entreprises.
Il y a de fortes chances que tester le scénario aurait aidé à trouver la faille.

2. 

Le bug est local, car touche uniquement une classe abstraite.

La liste stocke l'élément courant pour suppression/etc, le hasNext() vérifie s'il y a un suivant en parcourant la liste tant qu'il y a des "nulls", si à la fin du parcours le dernier élément est "null" (donc tous les autres aussi) retourne "false" (donc ok) et "nulltralise" les éléments courants (pas ok; effet de bord/modification imprévue), ce qui fait que un "remove" n'a pas d'effet vu qu'il veut enlever un "null" plutôt que le dernier élément renvoyé.

La solution est de supprimer la "nulltralisation" (imprévue).

Les contributeurs ont ajouté des nouveaux tests pour s'assurer que le bug soit détecté s'il réapparait dans le futur.

3. 

Les expérience concrète sont de "casser" aléatoirement un service pour voir si le système fonctionne toujours, prendre un groupe de personnes comme testeurs pour mesurer l'impact de l'erreur en comparant à l'aide d'un autre groupe qui ne subit pas la défaillance et introduire des défaillances ou de la latence dans les requêtes pour observer le comportement du système et sa réponse face à cela.
Les requis pour ces expériences sont les hypothèses, les variables indépendantes, les variables dépendantes et le contexte.
La variable principale observée est le SPS ((stream) starts per second) lors de leurs test.
Les résultats principaux obtenus sont des métriques qui leur permettent de savoir si une faiblesse a été détectée ou pas, puisque à présent les ingénieurs sont capable de deviner si la fluctuation montre des problèmes ou pas.

Netflix n'est pas la seule compagnie à faire ces expériences, il y a notamment Google et Amazon.

Cela pourrait être utile notamment pour des sites webs vendant des articles, en observant combien de personnes mettent des choses dans leurs favoris, achètent avec succès, parviennent à s'authentifier ou à créer un nouveau compte, ainsi que la latence entre les actions.

4. 

Une spécification formelle implique que l'on sait exactement ce que l'on est censé obtenir lors de l'utilisation du langage. Aussi, permet de savoir ce qui devrait exactement se passer lors de l'exécution d'un code, et de n'importe quelle propriété.

Non, cela ne veut pas dire qu'il ne faut pas les tester, les développeurs sont des humains et tout le monde commet des erreurs malgré tout les efforts fournis.

5. 

Les avantages principaux de cette spécification mécanisée est qu'elle est vérifiée et vérifiable, et respecte la spécification originale le plus possible. Elle a en effet permis d'améliorer le spécification formelle originelle du langage, car elle a notamment permis de dénicher des bugs qui seraient sinon restés introuvés, et donc ces bugs ont pu être corrigés. Ils ont vérifié leur spécification avec notamment leurs propres outils, comme leur interpréteur vérifié, leur vérifieur de type exécutable fait en Isabelle, et avec Isabelle. Cependant cela ne retire pas le besoin de tester. Leurs outils auraient notamment besoin de recherche plus approfondis pour en faire des applications capables de tourner seules.
