# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. [Eve Online's boot.ini deletion bug](https://www.kotaku.com.au/2007/12/eve_online_windows_xp_boot_bug/). 
After a patch was released, users found their computer's boot.ini file deleted. 
The game used a file also named boot.ini and the bug came from the path used to delete the file. 
During the installation, the file was deleted due to a faulty use of the specifications of the method. 
This bug is local as it impacts the client, and not every client were impacted as some configurations of Windows were 
capable of restoring the file. The most impacted clients would find themselves not able to boot their computer after the update.
In the [post](https://www.eveonline.com/news/view/about-the-boot.ini-issue) where they addressed the issue, 
they also mentioned the fact that they did not check that this behaviour did not happen and that they should have but 
did not because of time limitations while yes it should have been considering it is a critical Windows file.

2. The bug [278](https://issues.apache.org/jira/browse/COLLECTIONS-278) is about the put() and putAll() methods that do 
not update the contents if an internal attributs, resulting in a different returned value when accessed. 
This is a local bug and was solved and tests added.

3. Netflix injects failures, shutdowns servers, and checks that the systems degrades gracefully.
The technique of Chaos Engineering is aso used by Amazon, Google, Microsoft, and Facebook.
They monitor the number of streams started per seconds to have an evaluation of the health of their systems.
Same could be applied to web services by monitoring the number of requests per seconds, or the response time.

4. Using formal specification, it allows the verification of the language to be much smaller 
(they took the example of JVM Bytecode taking 150 pages, while WebAssembly fits in one).
Because a language is formalized does not mean the code should not be tested. 
For example is important to make sure that your code does what you want and only what you want. 

5. Using their interpreter allows to prove the code with reduction relation, which in turns allows to prove the specification
and have an optimised interpreter, that avoids performances pitfalls.
