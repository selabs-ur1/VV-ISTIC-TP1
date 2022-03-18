# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1) En l'an 2000, l'institut national du cancer de Panama se trouve client d'un logiciel de l'entreprise américaine Multidata System International.
C'est une société qui développe un logiciel de planification de la thérapie. Elle calcule la dose de rayonnement appropriée pour les patients soumis à une radiothérapie. En effet, ce logiciel permet aux radiothérapeutes de tracer sur un écran d'ordinateur des blocs (boucliers métalliques) afin de protéger 
les tissus des rayonnements. Cependant, alors que le logiciel n'était prévu pour dessiner que 4 blocs, les médecins ont détourné l'utilisation de ce système en réussissant à ajouter un cinquième bloc. Cinq blocs sont déssinés comme un seul gros bloc avec un trou au milieu. Le logiciel, lui, répond différement en fonction 
de la configuration et du sens du trou. Dans certains cas, la dose était correcte et dans d'autres, l'exposition nécessaire a été doublée. En 2001, cela a provoqué la mort de 8 patients 20 blessés susceptibles de développer des problèmes de santé. Les médecins ont été également condamnés pour meutre car ils étaient censés vérifier la dose à la main. Le logiciel aurotisait des formes incorrectes de données, ce qui a provoqué des erreurs de calcul des temps de traitement. Selon nous, ce bug est un bug plutôt global car ce sont des erreurs d'input d'une probable API qui ont provoqué des interactions inappropriées avec d'autres composants. Le bug aurait pu être probablement corrigé en vérifiant les données reçues. Des tests unitaires auraient pu être être suffisants pour le détecter.

2) 
    Problème : UnmodifiableNavigableSet peut être modifié par pollFirst() et pollLast(). 
    link issue : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-799?filter=doneissues
    
    Description de l'erreur : les deux methodes pollFirst et pollLast ne lèvent pas d'exception de type 
    '''UnsupportedOperationException``` sur un ```UnmodifiableNavigableSet```. 
    
    pollFirst() : cette fonction permet de recupérer et supprimer le premier élément (le plus bas) dans un ensemble d'éléments. renvoie null si l'ensemble est vide. 
    PollLast() : Cette focntion nous eprmet de récupérer et suppriemr le dernier élément (le plus haut) dans un ensemble d'éléments. Renvoie null si l'ensemble est vide. 

    La solution apportée : ils ont réimplementés les deux méthodes pollFirst() et pollLast() de l'interface '''NavigableSet``` et ils également rajouté l'exception ```UnsupportedOperationException``` dans les deux methodes. 
    Aussi, ils ont fait des tests, et dans les tests ils ont mis un try catch et une condition (if),  l'appel des deux methodes se fait sur un '''NavigableSet```. 

    Ici la reimplémentation des deux methodes pollFirst() et pollLast()
    ```java
      /**
     * @since 4.5
     */
    @Override
    public E pollFirst() {
        throw new UnsupportedOperationException();
    }

    /**
     * @since 4.5
     */
    @Override
    public E pollLast() {
        throw new UnsupportedOperationException();
    }
    ``` 

Test : 

    ```java
    if (set instanceof NavigableSet) {
            final NavigableSet<E> navigableSet = (NavigableSet<E>) set;

            try {
                navigableSet.pollFirst();
                fail("Expecting UnsupportedOperationException.");
            } catch (final UnsupportedOperationException e) {
                // expected
            }
            try {
                navigableSet.pollLast();
                fail("Expecting UnsupportedOperationException.");
            } catch (final UnsupportedOperationException e) {
                // expected
            }
        }
    }
    ```
    
3) Netflix utilise une technique assez répandue dans ses systèmes afin d'améliorer l'expérience utilisateur mais également de détecter des défaillances. Cette technique correspond au Chaos Engineering. Les ingénieurs injectent des défaillances dans des services non critiques et vérifient que celles-ci ont un impact minimal sur l'utilisateur. Afin de formaliser une expérience dans l'approche de l'ingénierie du chaos, il est nécessaire de formuler une hypothèse, varier les évènements réels, réaliser des expériences en production et les automatiser pour les faire fonctionner en continu. L'objectif est de comparer l'état stable du système et l'état expérimental afin de réfuter ou valider l'hypothèse de départ.
Par exemple, ils ont injecté une défaillance dans les signets de Netflix, celle-ci n'est pas sensée perturber la diffusion en direct. Cette défaillance aura un impact sur une partie des utilisateurs (groupe expérimental). Ensuite, ils comparent le SPS ("start par seconde" ou le nombre de personnes commençant à regarder une vidéo par seconde) entre le groupe contrôle (celui qui n'a pas la défaillance) et le groupe expérimental. Un changement inattendu du SPS pourra déterminer un problème dans le système. Cette métrique sert de principal indicateur de la sante globale du système Netflix.
D'autres entreprises utilisent des approches semblables comme par exemple AWS d'Amazon. Le problème est que ces entreprises ne documentent pas forcément leurs pratiques.

4)
 - What are the main advantages of having a formal specification for WebAssembly ?  
    WebAssembly est un fromat d'instructions binaire pour une machine virtuelle basé sur la pile, safe, portable, rapide, compact, avec une haute perfermance. 
    Le fait d'avoir une sémantique fromelle permet d'avoir une architecture aprouvée mathématiquement, uen architecture commune, permet d'éviter les bugs. 
 - In your opinion, does this mean that WebAssembly implementations should not be tested ? Tout doit être testé. Le code WebAssembly est exécuté, utilisé sur les machines des utilsateurs, les bugs peuvent provenir des utilisateurs, et peut être même des développeurs de WebAssembly. 


5) La concept de spécification mécanisée permet de construire un système de preuve sur un langage par l'intermédiaire d'interpréteur et de vérificateur. Cela est utile pour améliorer la spécification formelle du langage qui peut présenter des lacunes. Par exemple, l'auteur a réalisé une mécanisation complète avec Isabelle de l'exécution du langage WebAssembly et de son système de typage. Grâce à des lemmes et des théorèmes en Isabelle HOL, ils ont trouvé des lacunes dans la spécification du WebAssembly comme par exemples que les invariants énoncés dans la spécification étaient trop faibles ou encore que la méthode trap du langage pouvait provoquer un problème lorsqu'elle remonte la pile d'appel. D'autres artefacts qui dérivent de la spécification mécaniséee est par exemples CakeML ou Java Virtual Machine. Selon nous, cette nouvelle spécification ne permet pas d'oublier le besoin de tester qui permet de rassurer à la fois le développeur, l'auteur de la spécification mais aussi le client. Cependant, elle peut couvrir un certain nombre de tests qu'on ne pourrait faire de manière exhaustive.
