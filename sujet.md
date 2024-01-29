# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Bug de compatibilité entre Windows 11 et Oracle VirtualBox ([ticket Oracle](https://www.virtualbox.org/ticket/20536))

Lorsque Hyper-V et Windows Hypervisor sont activés sumaltanément, il n'est alors pas possible d'utiliser VrutalBox pour lancer une machine virtuelle car un message d'erreur s'affiche.
Ce Bug peut-être considéré comme global car il fait intervenir un logiciel et un système d'exploitation, et que l'un sans l'autre le bug n'est pas présent.

Une fois que Microsoft a eu connaissance de ce bug, ils ont empêché la migration de Windows 10 vers Windows 11 en bloquant la mise à jour si VirtualBox est installé sur la machine.

Les utilisateurs ayant déjà installé Windows 11 devaient désactivé au choix Hyper-V ou Windows hypervisor pour refaire fonctionner leur machine virtuelle.

Le problème a été réglé suite à une mise à jour de VirtualBox effectuée par Oracle, et les utilisateurs possédant cette mise à jour pouvait ensuite migrer vers Windows 11 s'ils le souhaitaient.

Ce bug aurait certainement pu être révélé en faisant les bons tests.  
Microsoft aurait pu faire plus de tests, mais il est compliqué de tester tous les logiciels dans toutes les configurations, où dans ce cas il faut que L'Hyper-V et Windows Hypervisor soient activés en même temps. Des tests automatiques semblent cependant difficiles à mettre en place côté Microsoft.  
Oracle a de son côté moins de configurations à tester et auraient donc pu faire plus de tests sur son logiciel lors de la sortie de Windows 11 pour s'assurer de sa bonne mise en place sur ce nouveau système d'exploitation.

source :  
https://lecrabeinfo.net/windows-11-liste-des-bugs-et-problemes-connus.html#problemes-de-compatibilite-avec-oracle-virtualbox  
https://support.microsoft.com/fr-fr/topic/kb5007125-suspension-de-compatibilit%C3%A9-lors-de-la-mise-%C3%A0-niveau-vers-windows-11-avec-oracle-virtualbox-install%C3%A9-ea84b50c-77f5-473c-99a8-81c2b7b53d35


2. UnmodifiableNavigableSet peut être modifié par pollFirst() et pollLast()

Ce bug intervient dans le package Apache Collection pour Java qui étend les fonctionnalités du package Java Collections. Il a été découvert qu'il était possible de modifié un UnmodifiableNavigableSet en passant par pollFirst() et pollLast() alors que ça devrait être impossible. Ces méthodes devraient lancer une UnsupportedOperationException comme c'est le cas dans le package Java Collections. 

Ce bug est donc un bug local car il ne fait intervenir aucune méthode extérieur au package. Pour le corriger il a donc été décider que ces méthodes devront lancer une UnsupportedOperationException lorsqu'elles rencontrent un UnmodifiableNavigableSet. Des tests ont également été ajoutés pour détecter que l'exception est bien lancée dans une telle situation.

sources :  
https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-799?filter=doneissues  
https://github.com/apache/commons-collections/pull/250


3. Chaos Engineering par Netflix

Netflix, en tant que service web est obligé d'ajouter des fonctionnalités régulièrement (plusieurs fois par jours) afin de rester compétitif et d'obtenir de nouveaux utilisateurs. Ainsi il ne peuvent pas prouvé tous leurs services à chaque fois. De plus, Netflix ne pouvant pas être placé sur un seul serveur, c'est un sytème distribué et donc un système complexe à tester. Ils utilisent alors un système de test qui s'appelle le chaos engineering. Le principe est simuler une panne quelque part dans le système et de voir comment il s'adapte à cette panne. Pour ce faire, Netflix a 2 logiciels qui tournent (chaos monkey et chaos kong) et qui permettent de stresser quotidiennement le système.

Pour vérifier l'adaptation du système, les ingénieurs regardent le nombre de personnes qui commencent à streamer chaque secondes, ou le nombre de personnes qui se connectent chaque secondes. Ils possèdent des courbes de normales en fonction de l'heure de la journée. Lors des tests, il regardent si les courbes dévient de la normale significativement. Plus les modifications résistent aux tests, plus les ingénieurs ont confiance en la capacité d'adaptation du système, au contraire, si une déviance est remarquée, cela montre une faiblesse dans le système.

Les différentes simulations de pannes peuvent être l'ajout de latence dans les requêtes, simuler un problème dans les communications entre services, l'extinction d'une machine virtuelle ou un nombre d'utilisateurs simultanés supérieurs aux attentes. 

Il sembleraient que des techniques similaires à celles décrites soient utilisées dans d'autres grandes entreprises telles que Google, Amazon, Facebook and Microsoft. Dans le cas de Google, il est possible de procéder un peu commen Netflix en regardant le nombre de recherhes par secondes, ou pour Amazon le nombre d'achats par secondes. Comme pour Netflix, une déviance dans les chiffres montre un problème sur la plateforme.

4. WebAssembly

Le langage WebAssembly à pour avantage d'être sécurisé, rapide, portable et déterministe. De plus, la représentation formelle permet d'être plus compacte et facile à décoder, facile à valider et compiler et d'être parallélisable. Il est donc plus facilement prouvable qu'un langage tel que javascript. Cependant, il est nécessaire de le prouver pour s'assurer de l'abscence de bugs potentiels.

Je pense que les programmes WebAssembly doivent quand même être testés afin de s'assurer que le programme fasse ce qu'il doit faire (validation). La vérification est normalement assuré par le langage, sauf dans le cas d'appels extérieurs (appels vers host), où là aucune garantie ne peut être donnée et doivent donc être testés

5. WebAssembly : preuve Isabelle

Au départ, WebAssembly était principalement spécifié à la main. Des erreurs peuvent alors être commises. Notamment, grâce à cette formalisation dans Isabelle, il a été possible de corriger certains problèmes que comportait WebAssembly, et donc d'améliorer sa spécification. Certaines propriétés n'étaient en réalité pas prouvées.

Les auteurs ont aussi créé 2 exécutables pour le langage : un vérificateur de type et un interpréteur. Ces deux exécutables sont spécifiés en fonction de la formalisation mécanique. Le vérificateur de type permet de vérifier les types en un seul passage mais ne permet pas une inférence de type complète. L'interpréteur mime la réduction qui est au coeur du langage

Cette formalisation ne permet pas de retirer complètement les tests, notamment les auteurs disent que le langage posssède des propriétés qui disent que le programme ne peut pas planter. Cependant, cela ne valide pas le programme, c'est à dire que ça ne dit pas si le résultat que l'utilisateur devait recevoir.