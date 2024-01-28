# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Bug : HeartBleed

The HeartBleed vulnerability exploits a bug in theOpenSSL library. The bug is in the implementation of the HeartBeat 
protocol. This protocol is used to check if the connection is still alive. The client send a message to the server and the server respond with the same message.

However, if the client send a message with a size smaller than the real size of the message, the server will send 
the message asked by the client and the data in the server memory until the real size of the message. The client 
will receive more data than asked and the gap between the message sent by the client and the message sent by the 
server will be filled by data in the server memory. That data could be confidential (password, files, etc...).

The extent of the damages caused by heartbleed is hard to evaluate. The bug was patched quickly after it was 
discovered but known to be exploitable for at least 2 years.

The bug is local, it is in the implementation of the HeartBeat protocol. The failure is a leak of data. The bug 
could have been discovered by testing the right scenario.

2. https://issues.apache.org/jira/browse/COLLECTIONS-796. SetUniqueList.createSetBasedOnList doesn't add list elements to the return value anymore. The documentation says it does, and it used to up to version 4.2, but a call to `addAll` was accidentally deleted. This is a local bug. The [PR] (https://github.com/apache/commons-collections/pull/255) brings back the call to `addAll` and add few tests to ensure the call will not be removed anymore.

3. Concrete experiments: Netflix conducts experiments involving various scenarios like terminating virtual machine instances, injecting latency into service requests, and simulating Amazon region outages to test system resilience.
Requirements: These experiments demand highly resilient systems capable of withstanding failures without significant disruptions to user experience.
Observed variables: Metrics like SPS (stream starts per second) are key indicators, assessing if failures in specific services impact critical metrics and if the system gracefully handles stress.
Main results: The paper doesn't detail specific experiment results but emphasizes the importance of system 
   resilience and degradation analysis during simulated failures. Most companies seems to use this method to test the 
   resilience of their systems.
  Speculation for other organizations: Experiments could involve scenarios relevant to their services, observing metrics like transaction completion rates or response times to assess system behavior and resilience. Variables observed should reflect critical aspects of the system's functionality and performance. Each organization would adapt Chaos Engineering based on its unique system requirements and user expectations.

4. A formal specification for WebAssembly ensures interoperable compatibility, enables rigorous validation and 
   verification processes, facilitates language evolution while maintaining consistency and erased all sources of 
   ambiguity. This does not mean that WebAssembly implementations should not be tested at all.

5. Mechanized specification ensure that two components have to be compatible with each-others and not rely on human 
   intervention. It doesn't improve the specification of the language, it only enforces it. As the program is proven, it doesn't need to be tested. 
