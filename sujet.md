# Practical Session #1: Introduction

---

## Question 1

Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

## Answer 1

We found a famous bug, described in an [article from *National Geographic*, written by *Morgan Stanley*](https://education.nationalgeographic.org/resource/Y2K-bug/), called the 'Y2K bug'. Before the year 2000, software programmers often used two digits to store the information about the year (as to save the other two there were assumed to always be '19'). This eventually became problematic as the year 2000 approached, since this assumption would stop being true.

This bug was pretty global, as the practice to hard-code the '19' part of the date was wide spread in the world.

Thankfully, the failure was pretty benign. In most cases, the bug manifested in dates showing up as being 100 years before the actual date. In one case though, it lead the information system of a nuclear powerplant in Japan to fail (the system was not properly sending information to a centralized server) due to the bug, but it too was quickly resolved.

The repercussions were overall small. In most cases, it lead to incorrect dates on information screens and other devices, and that, until the software were updated to store the date using four digits.

The problem could have been avoided easily. The year 2000 came with no surprise, and software programmers had years to prepare for this eventuality, and adapat their programs to store dates appropriately.

---

## Question 2

Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

## Answer 2

The bug [COLLECTIONS-799](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-799?filter=doneissues) was a bug raised on September 7th, 2021. This bug was related to `UnmodifiableNavigableSet` objects, where, `pollFirst()` and `pollLast()` methods could be executed without throwing any exception. These two methods were inherited from a parent class, so the fix simply consisted in overriding the methods, and throwing `UnsupportedOperationException` exceptions if they were called. The contributor that made the changes also thought to implement tests, in order to guarantee that there would not be regressions in the future leading to the return of this bug. The issue was local, as it was a programming error located in a precise area of the code-base.

---

## Question 3

Netflix is famous, among other things, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

## Answer 3

Netflix performs the following experiments as part of their *Chaos Engineering* practice :
- Terminate virtual machines instances;
- Inject latency into requests between services;
- Fail requests between services;
- Fail an internal service.

One of the primary requirements is that the experiments should not have a strong inpact on the quality of the real service. The primary variable they observe to quantify the impact of their experiments is the SPS - (stream) starts per seconds. Netflix is not the only company that practices Chaos Engineering, LinkedIn, Microsoft, Google, most of the major tech companies use it. Another domain where Chaos Engineering would be useful is for public transportation systems, notably, metro systems. Experimenting (or simulating) different failures could help identify major weaknesses in the said system, and prevent it from being unavailable for weeks.

---

## Question 4

[WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

## Answer 4

According to the research paper, having a formal specification is an advantage to have a complete, yet concise formal semantics of both execution and validation, including a proof of soundness. It also demonstrates
the "real world" feasibility of the approach, and it also leads to a clean design.

While the it has a formal specification, there could still be issues in its potential implementations, and as such, it should in our opinions be tested.

---

## Question 5

Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answer 5

The main advantages of the mechanized specification is that it preserves 'Eyeball closeness' (a line to line correspondance between the specification and the mechanisation), meaning the specification already include a definition in formal notation. By what was mentioned previously, this new specification improves the original one. An artefact resulting from this work, the author produced a fully mechanised proof of both soundness properties. No, the new specification, while being closer to the implementation (thanks to the Eyeball closeness), still requires to have a tested implementation.
