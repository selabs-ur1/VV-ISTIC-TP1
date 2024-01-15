# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers


1 : Nous allons parler du bug heartBleed, découvert en 2014.  
Lien de l'article : https://www.lemonde.fr/pixels/article/2014/12/30/faille-de-securite-heartbleed-le-pire-scenario-a-ete-evite_4547487_4408996.html  
Liens infos complémentaires :   
https://fr.wikipedia.org/wiki/Heartbleed   
https://www.synopsys.com/content/dam/synopsys/sig-assets/datasheets/Heartbleed-Story.pdf  
Heartbleed est une vulnérabilité logicielle introduite par un développeur bénévole dans la librairie openSSL.  
OpenSSL propose une implémentation open source du protocle SSL (secure socket layer).  
OpenSSL est très largement utilisée (66% des serveurs web), donc ce bug a une portée globale.  
Cette vulnérabilité a été découverte par des chercheurs de Codenomicon et Google.  
Elle a été découverte pendant que ces dernier amélioraient une fonctionnalité dans leurs outils de test.  
Il se sont aperçu qu'il était possible d'attaquer les serveurs de la compagnie (Codenomicon) depuis l'exterieur des locaux, sans laisser la moindre trace.  
Via cette faille, il était possible de voler des clés privées utilisées pour des communications sécurisées.  
Voici le scénario : Les serveurs communiquaient via deux canaux avec les clients(navigateurs).  
Le premier canal était celui d'échange, correctement chiffré par clés asymétriques, donc le flux est illisible sans possession des clés privées.  
Un deuxième canal dit "heartbeet", dont le rôle était de permettre au serveur de ping le client pour savoir si il était toujours présent (par exemple fermeture du navigateur).  
Le canal heartBeet n'était pas chiffré et donc vulnérable aux attaques.  
Les pirates utilisaient ce canal afin se faire passer pour le serveur pour d'obtenir des informations du client, lequel les fournissait sans broncher, car de son point de vue c'etait une requête tout  
à fait légitime provenant du serveur avec qui il était déja en communication.  
Tester le bon scénario implique de connaître l'existence de ces deux canaux et une connaissance approfondie de la librairie SSL.  
Cela aurait tout a fait pu être évité en chiffrant le canal heartBeet.  
Cette faille a très certainement eu de lourde conséquences (vol de données) car elle a été présente deux ans avant d'être corrigée.  



2 : Ticket choisi : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-802?filter=doneissues  
Le bug portait sur la manière dont les appels de méthodes sur un itérateur devaient affecter la collection à laquelle il était associé (ici une map)  
Le scénario souhaité était que si on est sur le dernier élément suite à des appel à iterateurSurMap.next(), puis que l'on appel remove(), on s'attend à ce que le dernier élément soit bien supprimé.  
Le bug se déclenchait si on appelait hasNext entre les appel next et remove, après être tombé sur le dernier élement.  
L'auteur tu ticket teste cela avec une map à une seule entrée, afin de tester si la map est vide après suppression du dernier élément (l'unique entrée de la map).  
Il s'avérait qu'un appel à hasNext quand on est sur le dernier élement suivi d'un appel à remove était faussé (dernier élément non supprimé).  
On peut considérer la portée du bug comme globale, les itérateurs sont des objets très largement utilisés par les developpeurs.  
Il est tout à fait probable que les itérateurs soient des sources de dépendances dans le projet apache.  
Cause du problème : quand on est sur le dernier élement, l'appel à hasNext renvoit faux, ce qui entraine une mise à null de la currentKey de l'itérateur (currentKey référence lélément courant).  
Ainsi l'appel remove revenait à essayer de supprimer l'élément null(suppression pour un pointeur currentElement pointant sur Null), ce qui ne correspond pas à la suppression de l'élément courant.  
Le bug a été corrigé en supprimant cette mise à null quand hasNext renvoit false.  
Il existait déja un test permettant de detecter ceci, c'est d'ailleurs ainsi qu'il a été découvert par un utilisateur éxecutant cette suite de test.  
Mais ce test ne provenait pas de la campagne de test de chez apache.(bug découvert via la Guava's testlib)  
Le test correspondant à été intégré chez apache. (ajout de la Guava's testlib au projet apache).  



3: Netflix dispose d'un système distribué sous forme de machines virtuelles hébergeant chacune un ou plusieurs services relatifs au systeme globale.(architecture micro-service)  
Le système global sur lequel une état d'équilibre constant est souhaité est le système distribué Controle Pane.  
Un équilibre est evidemment égalemment souhaité sur leur réseau d'hebergement et de distribution des fichiers videos (Open Connect), mais l'article se focalise sur Controle Pane.  
  
Concernant le principe d'une "experience" dans le chaos engineering il faut :  
- construire une hypothèse décrivant l'état d'équilibre d'un service comme mesurable (ici avec la métrique SPS)  
- Faire varier l'état du système (par exemple éteindre des vm hebergeant des services)  
- Executer ces experiences en production, certains scénarios ne sont productibles qu'en production.  
- Rendre ce procédé automatique, afin d'evaluer les impacts des modifications du système en production.  

Pour le cas de netflix, on s'intéresse à un équilibre sur la disponibilité de leurs services (principales liés à l'interface utilisateur).  
La métrique faisant office de référence dans leur scenario est SPS (nombre d'utilisateurs lançant une vidéo par seconde).  
Exemples de variations du système dans leurs experiences :   
- terminer des instances de machines virtuelles hebergeant des services.  
- injecter de la latence sur les requêtes adressées à ces services.  
- faire echouer certains services  
- Rendre toute une partie de leurs machines virtuelles inaccessible  
  
Le but de ces experiences est de simuler des pannes réelles afin d'observer leur impact sur la disponibilité des services qu'ils offrent.  
  
Netflix n'est pas la seule compagnie pratiquant le chaos engineering, il y a également amazon, google, microsoft et facebook (les seules citées, il y en a probablement bien d'autres)  



4: La mise en place d'une spécification formelle installe un standard commun, auquel toutes les implémentations doivent rigoureusement se soumettre, favorisant ainsi l'intéropérabilité.  
De plus, cette spécification peut etre vue, comme un "guide", ce qui aide lors de la conception d'une implémentation.  
Comme pour tout langage, une spécification formelle lève les ambiguités sur la sémantique.  
Une spécification formelle ne remplace pas le rôle des tests, l'un sert de directive, l'autre fournit un support de verification et validation pour l'implémentation.  



5:
La spécification mecanisée à permis de reperer certaines erreurs dans la specification de base, par exemple sur la propriété de regression.  
Elle a également permis une révision de la spécification sur l'opération return qui n'était pas facilement compréhensible.  
De part l'utilisation d'un outils de preuve, une spécification mécanisée vise à fournir des preuves formelles sur des propriétés d'une spécification.  
Ce travail à entrainé la création d'un typeChecker (on lui fournit en entrée un programme WebAssembly, il verifie que le typage y est correct) et d'un interpreteur WebAssembly.  
Bien que cette approche constructive ajoute de la robustesse dans la spécification, la présence de tests est toujours pertinente.  
On pourrait supposer qu'un nouveau test amènerait a découvrir une lacune dans la spécification, donc non cela ne remplace pas la nécéssité de réaliser des tests.  
  

