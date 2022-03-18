# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

https://docs.google.com/document/d/1FCyM0qA4kfsE2tfpRlrlFUPgvhS7E9ACNDNUxSeDwns/edit?usp=sharing

#### 1. Perturbation boursière de Knight Capital.
Au mois d'Août 2012 un bug a causé une nette augmentation de valeur boursière de cette entreprise. En effet, elle a vu son capital 
annoncé bien plus grand qu'en réalité suite à une mise en production des passages d'ordres.
Le bug est du à l'oubli de mise en production d'un code sur l'un des huit serveurs qui exécutaient alors automatiquement le calcul
de balance de valeurs. Ce bug <b>local</b> causa le plongeont de la valeur des actions de l'entreprise, la mettant pratiquement en faillite.
Détecté à cause de sa visibilité publique dans le millieu du traiding, une façon d'éviter ce genre de problème est d'automatiser le
déploiement et d'éviter ainsi l'erreur humaine. Les tests qui auraient pu être effectués sont de vérifier, avec un même jeu de données,
si les résultats de chaque serveur est identique.

#### 2. Nullpointer Exception mal spécifié
Le ticket Jira <a href="https://issues.apache.org/jira/browse/COLLECTIONS-516">COLLECTIONS-516</a> remonte un problème <b>local</b> majeur
concernant la gestion des pointeurs sur adresses vide lors de l'initialisation. Le ticket précise donc que la spécification n'est pas claire,
car cette dernière dit qu'initialiser à <i>null</i> amènera à <i>null</i>. Dans les commentaires, ont peut suivre l'évolution de la résolution
qui, partant d'une solution visant à tester le cas d'initialisation à <i>null</i>, fut amnené à spécifier plus clairement qu'une mauvaise 
initialisation amène conséquemment vers un excecption de type <i>NullPointer</i>.
Ce cas est intéressant car on aurait tout à fait pu gérer le cas en vérifiant la nullité des entrées, mais une meilleure documentation a été 
préférée.

#### 3. Chaos Engineering
Netflix utilise le <i>Chaos Engineering</i> pour tester ses service en introduisant des erreurs dans les serveurs par des variables potentiellement
réelle qui provoqueraient les pires cas : Crash de serveurs, disfonctionnement matériel (disques durs) et coupures réseaux.
Pour effectuer celà, il est necessaire d'avoir de bons outils de monitoring et un système d'injection d'évennements. En injecttant volontairement un
évennement critique, Netflix surveille grâce aux outils de monitoring que les services contournent le problème ou utilisent leur back-up.
D'autres entrrprises utilisent cette technique, par exemple Facebook avec Storm Project, la SNCF avec Day of Chaos ou encore Azure avec la plateform
Proofdock.
On pourrait tout à fait se servir d'un tel système pour des environnements connus tels que le site des impôts, Discord ou toute application destinée à
servir un très large publique et dans toutes les situations - dues à l'environnement géographique ou la limitation technologique.

#### 4. Web Assembly avantages et testabilité
L'avantage de <i>Web Assembly</i> est qu'il dispose d'une spécification formelle capable de gérer le fait que ce qui est codé doit absolument respecté les
contraintes décrites par une spécification préalablement renseignée.Ceci à l'avantage de nous assurer que ce qui est exécuté correspond au plus possible
à la fonctionnalité décrite par les spécification. On peut aussi vérifier du code dit <i>untrusted</i> ou prévoir l'attaque des codes téléchargés depuis 
certains sites. 
Cependant cet avantage n'est pas suffisant à écarter les tests. Bien sûr, il faut adapter ces tests en fonction. Par exemple, tester que le code respecte
les contraintes de la spécification est inutile puisque c'est le principal avantage de <i>Web Assembly</i>. En revanche, tester les cas d'erreurs (Vérifier 
qu'un exception est lancée, ou le traitement ignorée) et les résultats (cohérence avec la spécification <b>externe</b> au programme informatique) est 
toujours nécessaire.

#### 5. Spécification Mécanisé de Web Assembly
Selon le papier, l'automatisation de la spécification de <i>Web Assembly</i> permet de mieux répondre à un principe appelé <i>Eyeball Closeness</i>. Ce principe
relève la correpondance directe entre la la spécification officielle et le système de mécanisation. Avec ce système, quelqu'un d'assez aisé avec l'écriture des
spécifications peut très facilement lire le code automatisé. L'avantage majeur est en résumé d'automatisé la création de code à partir d'une spécification visuellement
agréable pour une personne humaine.
Celà n'améliore pas vraiment la spécification formelle originelle de <i>Web Assembly</i> mais garanti plutôt qu'elle soit correctement interprétée et codée.
Pour tester les spécifications, Il faut comparer le résultat produit par l'outil Isabelle et ce qui est généralement fait dans l'industrie, conséquemment il faut
des spécifications déterministes.
