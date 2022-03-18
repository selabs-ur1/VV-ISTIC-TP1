# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

### Answer

#### Description

Log4j security flaw in 09_12_2021.
Remote execute of code in any java program that use this library \< 2.15 & JNDI.

#### Scope

Local issue, since it's caused by the code in the application.

#### Repercussions

Numerous applications worldwide are vulnerable.
Update needed for the library.

#### Potential solution

Attacking w/ red team could have been a way to test & discover the issue.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

#### Description

https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-799?filter=doneissues
UnmodifiableNavigableSet can be modified by pollFirst() and pollLast().

#### Fix

Exception throw when the user tries to modify the set.
New [tests](https://github.com/apache/commons-collections/pull/250/commits/18bb8734d69b3729402bcae8f0e5a12fdc80774d) have also been added to prevent regression.

3. Netflix is famous, among other things we love, for the popularization of _Chaos Engineering_, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

### Concrete experiments

Simulate failures by taking down random services.

### Requirements

Running experiments :

- building hypothesis
- select variables to change (latency, fail, terminate machines, ...)
- Run in production
- Automate experiments to monitor system

### Variables they observe

SPS, start per seconds.
How many users are starting a stream per second.

### Main results

if fail:

- Decrease in SPS, server and clients errors.  
  If OK:
- Constant metric

### Other companies doing it

Amazon, Google, Microsoft, Facebook

### How to replicate in other organizations ?

- Already have or create metrics of reference
- Create hypothesis which could cause service disruptions and a decrease of the metric.
- Varying real-world events
- Run in Production
- AUtomate and analyse results

Ex: Google on Youtube's continuous service.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested?

### Main advantages of having a formal specification

Better understanding of the language for internal & external developers.
Tests will be easier.

#### Should be tested ?

Yes, implementation could differ from the paper.
Could be proven that code in input is doing the same thing as the code in output.
What can't be proved should be tested.

5. Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

Eyeball closeness between WebAssembly specs and IsabelleHOL ?

From paper:
"Eyeball closeness is a design principle of the formal
model such that there is a line to line textual correspondence
between the official specification and the mechanisation. In
the ideal case, someone familiar with the official specification"
