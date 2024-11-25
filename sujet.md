# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

#### Question 1:
Article : https://www.lemondeinformatique.fr/actualites/lire-une-entree-de-trop-a-l-origine-du-bug-de-crowdstrike-94462.html

CrowdStrike is an antivirus with a lot of privileges on its clients computers. After an update, a bug in the software caused it to crash resulting in a blue screen on all the Windows computers where it got updated. It can be considered as global.
The bug report states that a buffer overflow was the cause of the crash. CrowdStrike does have testing procedures, but they passed different versions of the software to two differents test pipelines : "Content Validator" and "Content Interpreter".
According to Microsoft, 8.5 million computers where affected by this bug. The clients were unable to use their computers and even worse, they couldn't apply a fix by themselves as it was rather technical.
Clearly testing the right scenario would have discovered the fault. Maybe code testing coulnd't uncover the issue, but just updating and launching the software on a computer would have been enough to see the bug.

#### Question 2:
Issue : https://issues.apache.org/jira/browse/COLLECTIONS-813

The bug was a local one. In the `CollectionUtils` class, the method `retainAll` wasn't throwing the `NullPointerException` (NPE) expected. The documentation states that it would throw an NPE if any one of the parameters is null. However, when using the method with a null param it would execute normally.
The solution was to add a test on each of the parameters, using the `Objects.requireNonNull()` method, at the start of the method. Two new tests were added.

#### Question 3:
Netflix performs multiple types of "Chaos" experiments. For example :
- Terminating virtual machine instances
- Injecting Latency into requests between services
- Failing requests between services
- Failing an internal service
- Making an entire Amazon region unavailable

For each of these experiments, a control group and an experimental group is defined. They observe the "steady state" of the metric they want to observe, often Stream Starts per Second (SPS). They then perform the experiment and observe the same metrics. If the metric is not in the same range as the control group, they consider the experiment a failure.
They can also observe other metrics such as CPU Utilization, or request latency. These metrics are expected to vary during the experiment, and only serve as proofs that the application is operating in degraded mode.

Netflix is not the only company performing "Chaos engineering". Other companies such as Amazon, Google, and Microsoft are also performing these experiments. Any company operating a large-scale distributed system would benefit from this type of testing.
The authors give some examples of metrics that could be observed by other organizations. They say a metric for an e-commerce application could be the Sales per second. For a social network, the metric could be the number of likes per second.
The experiments that could be carried vary depending on the system. For example, a social network could test the system's ability to handle a large number of account creation. An e-commerce application could test its ability to handle the failure of order confirmations.

#### Question 4:
Because of its formal specification, WebAssembly has standard soundness properties. This means a valid program written in WebAssembly cannot have an undefined behavior during its execution.
A concrete example of what it means is the absence of type-safety violation or access to code addresses. WebAssembly guarantees memory safety.
However, it is not because these types of bugs can't exist that the code shouldn't be tested. The inability to write wrong code doesn't mean one can't write bad code. Tests do not only seek the absence of bugs, their role is also to check the correct behavior of the application. Unit tests and fonctionnal tests should still be required.

#### Question 5:
Thanks to the work of the authors, the mechanized specification of WebAssembly is more precise and more detailed than the original formal specification. The author provides an executable interpreter and an executable type checker.
While attempting to prove WebAssembly's soundness, the autor uncovered some errors in the original specification. The errors were fixed by the working group behind WebAssembly, and the specification was improved.
Using the draft specification of WebAssembly from 2017, the authors produced a full Isabelle mechanisation of the core execution semantics and type system. This mechanised specification only covers programs post-instanciation (instances). The authors made sure to respect Eyeball closeness, which is a design principle that aims to ease the comparison between the offical specification and the mechanisation.
Just as explained before, even if the language is perfectly safe and can't throw an unexpected error, the behavior of the application should still be tested. However, this mechanised specification allows code writers to be more confident in the reliability of their programs.