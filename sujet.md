# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### Question 1

**Pentium Division Bug:**

**Description:** The Pentium Division Bug was a precision issue in the floating-point unit of the Intel Pentium processor, specifically the original Pentium (P5) launched in 1993. Discovered in 1994, it impacted the processor's ability to perform division accurately beyond the ninth decimal place.

**Scope:** Local, because it's an error about division

**Manifestation:** While calculation errors were rare, researchers and users highlighted inconsistencies in division results under specific conditions.

**Repercussions:** The extent of the problem was debated, as errors were unlikely in typical use cases. However, Intel's initial response downplayed the issue, leading to controversy. Eventually, Intel initiated a processor replacement program under public pressure.

This incident underscored the importance of transparency and communication regarding software and hardware defects, prompting companies to adopt more open policies concerning product issues. It also had significant implications for quality management and testing procedures in the semiconductor industry.

### Question 2

https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-814?filter=doneissues

Dans cette issue, on retrouve un problème d'appel de fonction avec de mauvais paramètres, qui ne lève pas d'exception de pointeur nul alors qu'elle doit en lever une. 
Pour le corriger, le développeur a rajouté deux conditions de vérifications que les deux paramètres ne soit pas nuls, et a rajouté un test unitaire pour vérifier que l'exception est bien levée.

### Question 3

Concrete experiments :
- Chaos Monkey : Randomly selects servers and shuts them down during business hours so that engineers can test their ability to detect and respond to failures.
- Chaos Kong : simulate the failure of an entire AWS region and verify that services are resilient to region failures.
- Failure Injection Testing : injects faults into a system to test the system's ability to handle them. 

The requirements are that they have to test during production because they can't fully reproduce all the system. They need to put real-worlds events in their tests. They automate the tests to be able to run them continuously.

Engineers observe SPS (Starts Per Second) and if there is an unexpected drop, they know that there is a problem.


Other big companies that use Chaos Engineering are Amazon, Google, Facebook and Microsoft.


### Question 4

The main advantages of having a formal specification for WebAssembly are safety, sast low-level code, portability and compact code. Despite the fact that WebAssembly is a low-level language, it is still a programming language and it is important to have a formal specification to avoid bugs and to be able to test it. 
