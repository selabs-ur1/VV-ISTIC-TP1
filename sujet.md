# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. A very know bug is the bug of the year 2000. This bug was a bug related to the formatting and storage of calendar data. This bug was a global bug because he can affect an entire system who is depedent of the date. This bug was discovered before it happens and if it was not discovered, many system all over the world was going to crash at the same time. A right scenario of test, is a formally test which test for every date format, the good behavior of the system.

2. The following bug : https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-817?filter=doneissues is a local bug which concern a transmission method, so this bug doesn't affect the entire system. The method take in parameter a precise value and return an imprecise value. A solution provided for this bug is to delete the method because it is unecessary. There is no mention of a test for this type of bug.

3.  Netflix is using chaos engineering to test their system, they modify an environnement variable of their system and look the behavior of the system or the behavior of the user who using the system. They are not alone to use this type of experiments, for example the SNCF is using chaos engineering for their system. They can for example simulate a breakdown of electricity and look the behavior of users or the behavior of the system.

4. The main advantages of a language like WebAssembly are the following :

    - Safe : prevent other programs to attack
    - Fast : C++ compiler optimized because of low-level code
    - Portable : avaible on different platform and browser
    - Compact : Compact the code to reduce load

5. Mechanized specification provide a better formalization for the language : there is more accuracy. This is also more easy to identify potentials errors.

The specification is verified with a proof language like Isabelle.

The test is still important, the language is more accurate but need to be tested.

