# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5. Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?


## Answers

## Question 1 : Crash du Mars Climate orbiter 1999

En 1999 une sonde envoyée en orbite sur mars s'est désintégrée en entrant dans l'atmosphère.
La source de ce crash est dûe à un bug en lien avec une erreur d'unité pour le calcul de l'impulsion des propulseurs.
En effet le logiciel fournit par Lockheed Martin produisait des résultats en livres/secondes (unité américaine) au lieu de fournir des valeurs en newtons/secondes. Ce bug correspond à un bug global puisqu’il est le résultat d’une mauvaise interaction entre deux morceaux de code fonctionnant correctement, seulement, les deux morceaux de code ne travaillent pas avec les mêmes unités. Ce bug a été mis en évidence par une déviation anormale de la trajectoire de la sonde supposée être en orbit à 224 km d’altitude mais qui se trouvait à 150 km d’altitude. Sa décente ayant été trop abrupte, la sonde n’a pas eu le temps de ralentir et est arrivée trop rapidement dans une atmosphère trop dense, entraînant sa destruction. Ce bug aurait pût être évité avant même les phases de tests si les spécifications d’utilisations du code avaient été rédigées / explicitées / et pris en compte. Il aurait aussi pu être détecté lors de scénarios de test car on aurait pu déceler un écart de valeur entre une impulsion attendue et celle fournie par le code bugé. 

[Lien vers l'article](https://www.simscale.com/blog/2017/12/nasa-mars-climate-orbiter-metric/)

## Question 2 :

Nous observons ici un bug local générant une IllegalStateException.
L’origine est liée à un problème avec la méthode EntryIterator.remove() dans la classe java Flat3Map. Dans cette méthode, il fallait d'abord appeler currentEntry.getKey(), puis currentEntry.setRemoved(true).
Ce bug a été fixé lors d’un commit, en échangeant simplement les 2 lignes.

En même temps que la correction de cette erreur, les équipes d’Apache ont ajouté un test concernant la fonction remove de la classe Flat3Map parcourue grâce à un itérateur.

Lien vers l’issue en question : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-734?filter=doneissues
Lien vers la correction du bug sur github : https://github.com/apache/commons-collections/pull/115/commits/37a0381f43e04ed50ef32c26e1bb5752011e56fc

## Question 3 :

Netflix utilise un service appelé Chaos Monkey pour couper de manière aléatoire des machines virtuelles gérant des services de leur infrastructure. L’objectif étant de mettre à l’épreuve la résilience de leur infrastructure pour toujours être capable de fournir un service, même s' il doit être dégradé. Cette philosophie à aussi été appliquée pour simuler le crash de serveurs Amazon utilisé par Netflix, ou encore l’injection d’erreur sur les services Netflix, afin de s’assurer que le système assure ses services en mode dégradé. Le métrique utilisé par Netflix pour quantifier l'impact de ces actions est le SPS (stream start per second) qui correspond au nombre de lancement de vidéos par les utilisateurs à la seconde. En effet, il s'est révélé qu’il représentait bien le niveau de santé de leur services.

L’article révèle que d’autres entreprises comme Amazon, Google, Microsoft et Facebook utilisent aussi le chaos engineering pour tester leurs infrastructures et leurs services. On pourrait imaginer tester les systèmes de l’ISTIC en coupant un des serveurs s’occupant de la gestion des mails, et nous nous reposerions sur le nombre de mails envoyé à la minute pour quantifier l’impacte de cette coupure.

## Question 4 : Avantages d’avoir une spécification formelle pour WebAssembly :

Pour WebAssembly, comme pour les autres projets dont l’enjeu est grand et le nombre d’utilisateurs important, une étude formelle aussi réfléchie et complète permet la conformité des projets et facilite l'explication et la compréhension, sans compromettre la facilité de décodage, la portabilité, la sécurité et l’efficacité de vérification.
Plus la grammaire d’un langage est affinée, moins on observe d’apparitions de défauts dans le système qui peuvent engendrer des défaillances à l’exécution. Connaître ce qui est attendu, et ce qui ne l’est pas, permet de valider des programmes qui passent au moins les règles syntaxiques.
Pourtant, les règles ne peuvent pas toutes êtres pensées et définies à la base du fait de la difficulté pour les créateurs de penser en dehors de leurs propres scénarios d’utilisation. De plus, le code récupéré sur le web peut provenir de sources non fiables. Cette méthode est forcément efficace et beaucoup de programmes seront écartés, mais malgré ces efforts de pensée, il restera toujours des oublis, des règles non définies qui pourront être exploitées et laisser passer des programmes malveillants.
Donc même en partant du principe que le cahier de spec donnant l’intégralité du langage est bien pensé, la couche de sécurité supplémentaire que procurent les tests est significative et quasiment indérogeable.


## Question 5 :
Selon l’auteur l’utilisation de leur interpréteur permet de prouver que le code est sain par rapport à la relation de réduction, permettant ainsi de disposer d’une spécification prouvable ainsi que d’un interpréteur optimisé et évitant les pertes de performances. Cela permettrait donc d’améliorer la spécification du langage.

Leur modèle a été validé en passant les tests de conformité du langage de base disponible dans le dépôt WebAssembly. Des tests différentiels de l’interpréteur avec des moteurs Web-Assembly majeur ont aussi été effectués, permettant de valider l'interpréteur mais aussi de découvrir des bugs sémantiques dans les WebAssembly commerciaux. Cela a notamment permis de trouver une erreur dans le binaire, erreur qui n’avait pas été envisagée, et qui a été corrigée par la suite, mettant ainsi en évidence la nécessité de continuer de réaliser des tests.
