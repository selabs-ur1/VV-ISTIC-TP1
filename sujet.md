# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. There are plenty of bugs in the first months of a newly release game, by browsing the web for 5 minutes, i can find [this article](https://gamerant.com/elden-ring-floating-invisible-horse-bug-video-clip/) for example, about a bug discovered in Elden ring, where a player managed to unintentionnaly fly an get stuck in the air with his mount. This bug seems local and is probably due to a game lag, wich makes it hard to reproduce and fix, the presence of this bug make the character in an illegal state, wich is fortunatly spotted by the game, and the response is just to kill the character.
The reprecussion for the client is a bad experience, which can lead into less sells for the company.
In my opinion, this bug could've been fixed with more testing, especially by creating some lags and incorrect input sequences.

2. The bug i choose is [COLLECTIONS-799](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-799?filter=doneissues). It is a local bug since it affect the object itself.
The bug is two function of the superclass worked on this object, but shouldn't. It was fixed by returning an exception when those two functions are beeing called.
The author modified the previous test case to make it fit.

3. The performed experiments made are checking bugs by randomly create outages, for instance, they can inject latency between requests, intentionnaly fail requests, or even make a whole region unavailable.
The requirements for this kind of experiments is to have strong datas on a steady state, with known measures, before creating an experimental state.
They observe some variables such as STS (starts a seconds) wich reflect of many streams are started by seconds, or even acceount created per seconds.
The results obtained are he difference between the steady state and the experimental stats on values of variables, if both states are the same, there is no issues, or at least the issue is taken care of by the system.
Other worldwide organization run this type of test, such as Google, Amazon, Microsoft, Facebook.
These experiments can be carried to any type of organization by simulating unexpected usages of softwares while watching how many bugs are found, with a tool like sentry. It will prevent the real user to see it.

4. The main advantage of having a formal specification is to have a mathematicly proved architecture, wich means the only possible bugs are from humans.
It still needs to be tested because it is never free of human made bugs, the implementation still can be failed by developpers.

5. The main advantage of the mechanized specification is forcing developper to be explicit with his asumptions, wich reduce the bugs made by human.
It helps improving the original formal specification since it's verified by a computer, wich is faster and can easily check for every evolutions made in the future.
There is also some artifacts that benefits from the mechanized specification, such as an executable type checker and executable interpreter.
The author verify the specification by running the executables type checker and interpreter.
This new specification reduce the implementations errors quite a lot, but humans errors are not handable, so the need for testing still exist.
