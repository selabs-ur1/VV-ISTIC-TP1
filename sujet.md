# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### 1.

#### Le bug d’Ariane 5 


Le 4 Juin 1996, le vol de la fusée Ariane 5 (vol inaugural) n'a duré que 37 secondes environ avant qu’elle ne quitte sa trajectoire et n'explose. La panne a été causée par la perte totale des informations de guidage 37 secondes après le début de la séquence d'allumage du moteur principal. Cette perte d'informations était due à des erreurs de spécification et de conception dans le logiciel du système de référence inertielle (SRI) . En effet, à une altitude d’environ 3500 m, une opération de conversion ( flottants 64 bits  vers entiers 16 bits) n’a pas pu être faite car la valeur entière de l’accélération horizontale de la fusée étant 5 fois plus forte que pour Ariane 4 excédait ce que peut exprimer un nombre entier à 16 bits. Ces instructions n’étaient pas protégées. Cette mauvaise conversion a alors provoqué l’envoi de signaux incorrects de déviation de la commande des tuyères des moteurs d’Ariane 5 ce qui a entrainé une augmentation de l’angle d’attaque et des charges hydrodynamiques sur la structure et par conséquent la séparation du booster puis l’autodestruction de la fusée.

Ce bug a entrainé une perte de 7 milliards de dollars pour dix ans de développement et une cargaison évaluée à 500 millions de dollars.

Les tests au niveau du SRI ont été menés de manière rigoureuse en tenant compte de tous les facteurs environnementaux. Mais, aucun test n'a été effectué pour vérifier que le SRI se comporterait correctement lorsqu'il serait soumis à la séquence de décompte et de temps de vol et à la trajectoire d'Ariane 5. Si un tel test avait été effectué, le mécanisme de défaillance aurait été exposé.

_Tirée de_ : [ARIANE 5, Flight 501 Failure,1996](https://www.csm.ornl.gov/~sheldon/cs422/ariane5fr.html)



### 2. 
[The issue](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-734?filter=doneissues) : The code will generate an IllegalStateException.
The reason for this problem is that there is a problem with the EntryIterator.remove() method in the Flat3Map java class.
```
public void testEntrySet() {
        final Flat3Map<Integer, String> map = new Flat3Map<>();
        map.put(1, "A");
        map.put(2, "B");
        map.put(3, "C");
        Iterator<Map.Entry<Integer, String>> it = map.entrySet().iterator();        Map.Entry<Integer, String> mapEntry1 = it.next();
        Map.Entry<Integer, String> mapEntry2 = it.next();
        Map.Entry<Integer, String> mapEntry3 = it.next();
        it.remove();        assertEquals(2, map.size());
    }
```

[The solution](https://github.com/apache/commons-collections/pull/115): In EntryIterator.remove() method, it should call currentEntry.getKey() first, then call currentEntry.setRemoved(true).
```
       if (currentEntry == null) {
                throw new IllegalStateException(AbstractHashedMap.REMOVE_INVALID);
            }
            
            parent.remove(currentEntry.getKey());
            currentEntry.setRemoved(true);
            
            nextIndex--;
            currentEntry = null;
        }
```


The bug is local. The bug exists in the method EntryIterator.remove() in Flat3Map java class, where the order of calling two methods is inverse, which results in the call of EntryIterator.remove() method generating an IllegalStateException. The solution is calling the method currentEntry.getKey() before the method currentEntry.setRemoved(true).
A new test testEntrySet() was added to ensure that the bug is detected if it reappears in the future.

### 3.

Ils ont identifié les quatre principes définissant le chaos engineering notamment une hypothèse sur le comportement en régime permanent, la variation des évènements du monde réel, l’exécution des expériences en production et leur automatisation. 

Le SPS((Stream) start per second) a été utilisé comme métrique caractérisant le comportement en régime permanent du système. Comme variation des évènements du monde réel, ils ont utilisé un certain nombre comme par exemple l’arrêt d’instances de machines virtuelles, l’injection d’une latence dans les demandes entre services, l’échec de requêtes entre services et l’indisponibilité des services dans une région entière. 
Ces expériences se font sur un certain nombre d’utilisateurs qui constitueront le groupe expérimental. Il y’a en parallèle un autre groupe de contrôle qui n’est soumis à aucune défaillance. Le SPS, considéré au départ comme étant équivalent dans les 2 groupes, est mesuré à la fin dans chacun des groupes.En fonction des résultats obtenus, on peut comprendre le comportement du système lors d'une certaine défaillance.

 Netflix n’est pas la seule entreprise à effectuer ces expériences. On a Amazon, Google, Microsoft et Facebook qui les font aussi.  Ces entreprises peuvent adapter ces expériences en fonction de leurs besoins, de leur infrastructure. 
 
 ### 4.
The main advantages of having a formal specification for WebAssembly are :
- safe to execute: Code must be validated to be executed safely. Validation rules are defined as a type system which has standard soundness properties.
- fast to execute: WebAssembly uses binary format which allows a browser engine to minimize page-load latency and parallelize compilation of consecutive function bodies.
- deterministic and easy to reason about: The deterministic semantics is given to corner cases such as out-of-range shifts across all hardware with only minimal execution overhead.
- simple interoperability with the Web platform: For embedding a WebAssembly implementation into an execution environment, it is the embedder who defines how modules are loaded, how imports and exports between modules are resolved, provides foreign functions to accomplish I/O and timers, and specifies how WebAssembly traps are handled.

In my opinion, it does not mean that WebAssembly implementations should not be tested because the specification itself should be checked and verified.

### 5.
According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

- The main advantages of the mechanized specification:

For the semantics of WebAssembly, the mechanized specification defines 65 reduction rules. A cleaner interface is provided for code extraction and host function behaviours that the original specification did not support are implemented. 
Concerning the model, the mechanisation adheres as strictly as possible to the ideals of "eyeball closeness". 

- Did it help improving the original formal specification of the language?

Yes, il helped improving the original formal specification of the language. The mechanized specification which models the mechanism by which a WebAssembly implementation interfaces with its host environment exposes a deficiency in the WebAssembly specification that undermined the soundness of the type system.

- What other artifacts were derived from this mechanized specification? 

The other artifacts derived from this mechanized specification are a separate verified executable inter oreter and type checker.

- How did the author verify the specification?

The specification is verified with their core proofs of correctness which uses their executable interpreter, both by using the official WebAssembly conformance test suite, and by conducting fuzzing experiments.

- Does this new specification removes the need for testing?

No, this new specification does not remove the need for testing because this new specification can reveal some deficiencies in the old specification but not all of them.


