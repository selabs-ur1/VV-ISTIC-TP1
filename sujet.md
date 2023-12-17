# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

---

PONCET Clara
LE COANT Etienne
G2 M2 ILa

## Answers

### Question 1

Source : https://www.tf1info.fr/conso-argent/video-impots-bruno-le-maire-reconnait-un-bug-sur-la-taxe-d-habitation-comment-des-avis-ont-ils-pu-etre-envoyes-a-des-enfants-2277135.html
This article describes a bug that happened a few days ago. People started receiving invoices for French residence taxes even though they were not eligible.
The government commented on this issue, stating that it was a software anomaly.
This bug has had little repercussions. The public administration (DGFIP) stated that millions of people had been affected across France but that they would get a refund if they had already paid.
While this bug did not have any major economical repercussion, it tarnished the public image of the institution.
According to us, this bug could have been easily avoided with proper testing as this specific tax no longer exists and should have never been sent out.

### Question 2

We have chosen the following issue: [COLLECTIONS-796](https://issues.apache.org/jira/browse/COLLECTIONS-796?jql=project%20%3D%20COLLECTIONS%20AND%20statusCategory%20%3D%20Done%20AND%20type%20%3D%20Bug%20%20ORDER%20BY%20updated%20DESC). The `SetUniqueList` class is an implementation of the `Set` interface which is designed to store ordered elements. The `createSetBasedOnList` method takes a `Set` and a `List` as arguments and returns a new `Set` containing the elements of the `List` argument. The returned `Set` is the same type as the input `Set` argument (e.g., `HashSet`, `TreeSet`, etc…).
This issue was created because the method would always return an empty `Set`. This was easily fixed with the addition of a missing line of code (`subSet.addAll(list)` before the return statement).

```java
protected Set<E> createSetBasedOnList(final Set<E> set, final List<E> list) {
    Set<E> subSet;
    if (set.getClass().equals(HashSet.class)) {
        subSet = new HashSet<>(list.size());
    } else {
        try {
            subSet = set.getClass().newInstance();
        } catch (final InstantiationException ie) {
            subSet = new HashSet<>();
        } catch (final IllegalAccessException iae) {
            subSet = set.getClass().getDeclaredConstructor(set.getClass()).newInstance(set);
        } catch (final InstantiationException
                | IllegalAccessException
                | InvocationTargetException
                | NoSuchMethodException ie) {
            subSet = new HashSet<>();
        }
    }
    subSet.addAll(list);
    return subSet;
}
```
This is a local bug since the implementation of the method did not meet the specification's requirements as it would always return an empty `Set` or throw an exception.
No new tests were added since the missing line had been deleted by mistake at some point so the tests probably already existed.

### Question 3

These experiments aim at comparing a distributed service in a steady state and the same service in a disrupted state where the optimal running conditions are not met. 
The first requirement is to define what the "steady state" should be and how the service should behave in this state.
Then we inject variables to simulate real life events such as a server crash, a hard drive failure or a network malfunction to see how the service reacts (ideally, it handles those events as gracefully as possible for the end user). 
They use the "starts per second" variable to measure the availability of content during these events and ensure that the end users are not impacted by them and are able to watch content as they normally would.
The two different outcomes of these experiments are either an increased confidence in the system if the tests have been successful, or vulnerability detection if the tests failed. Either way, running this type of tests is a very positive action.
Other companies (mostly GAFAM) use similar techniques to test the resilience of their services.
Theoretically, any service provider that has several physical servers could use this kind of tests. We could simulate the crash of a server and see if the other ones are still serving content to the clients. We could simply monitor the load on each remaining server during the experiment to make sure the end users are not affected by the crash (no noticeable slowdowns for the users, no timeout or unavailable content, etc.).

### Question 4

Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested?

WebAssembly being fully specified makes it easier to validate code before running it since the semantics of the language are clearly bounded by simple rules (unlike JavaScript which has dynamic typing for example). Testing is still required as the implementation of the specification may be wrong and/or incomplete.

### Question 5

According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing

This mechanized specification was made with the Isabelle proof assistant. It helped find shortcomings in the original specification. For example, it proved that the specification for exception propagation was wrong to some extent, as in some cases, exceptions might have not be propagated as intended. This still does not remove the need for testing. Specific implementations for that specification might still be incorrect.
