# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. https://techcrunch.com/2023/05/01/twitter-is-randomly-logging-out-users-youre-welcome/
The bug affecting Twitter seems to be a forced logout issue.
Where users are browsing the Twitter website, the page refreshes, and they are unexpectedly logged out.
The bug seems to have a global impact, affecting desktop users.
Repercussions for Clients/Consumers and the Company:
It results in an interrupted service, making it challenging for the clients to access and use the platform.
The article notes complaints on Twitter, indicating user dissatisfaction.
The company, Twitter, may face negative publicity and a potential decline in user trust and satisfaction.
These issues could impact Twitter's reputation and user engagement.
In my opinion, testing the right scenarios could have helped discover this fault before it reached users.
The bug's origin, mentioned as a "bad front-end deployment," suggests that thorough testing of the deployment process could have prevented this issue.

2. This bug :
https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-766?filter=doneissues
is a local bug as it concerned Java 11 users and above.
It seems like there was an issue with the build on JDK 11+ due to the addition of the Math.floorMod(long, int) function,
which is not available in JDK 8. The contributor, @XenoAmess, addressed this issue by hiding it in a test file through force casting to long.
Additionally, there was a javadoc error fixed in one of the commits.
There was also a discussion about the use of --no-transfer-progress in the Travis CI scripts.
The contributor mentioned that it caused trouble only on JDK 8 on ppc64le and proposed either using an if condition based on JDK version or deleting --no-transfer-progress.
After some testing, they concluded that both options are acceptable, with a slight preference for deleting --no-transfer-progress.
The changes were reviewed by @kinow, who tested them on Ubuntu with JVM 8 and 11, confirming that the build worked with no issues.
The changes were approved, and the contributor closed the pull request after squashing the commits into a single one.
As for adding new tests, it's not explicitly mentioned in the provided information.
It's essential to ensure that new tests are added when addressing bugs to prevent regressions in the future.
However, without specific details about the existing test suite, it's challenging to determine if new tests were added in this case.

3. 
**1. Concrete Experiments:**
- **Chaos Monkey:** Random termination of virtual machine instances during working hours to encourage engineers to design services that can withstand individual failures.
- **Chaos Kong:** Simulating the failure of an entire Amazon EC2 region.
- **Failure Injection Testing (FIT) Exercises:** Causing requests between Netflix services to fail and verifying graceful degradation.
**2. Requirements:**
- Well-defined and isolated testing environment.
- Monitoring tools to track the impact of experiments.
- Automated systems for fault injection and experiment orchestration.
**3. Variables Observed:**
- Response time of the system under different fault scenarios.
- Error rates and error handling capabilities.
- Resource utilization during and after simulated faults.
- System recovery time after a failure.
- Metrics like SPS (stream starts per second) to assess the overall health of the system.
**4. Main Results:**
- Engineers at Netflix design services to handle failures routinely.
- Improved reliability and fault tolerance. 
Chaos Engineering is part of a discipline adopted by other tech organizations like Amazon, Google, Microsoft, and Facebook.
**5. Speculation on Other Organizations:**
- Other organizations could perform similar experiments tailored to their technology stack and infrastructure.
- Experimenting with different failure scenarios such as database outages, third-party service failures, or security breaches.
- Observing variables like database response times, API call success rates, and security event logs.
- Customizing experiments to reflect specific business priorities and potential failure modes.

4. The paper discusses the advantages of having a formal specification for WebAssembly and outlines the design goals and features of WebAssembly. The main advantages of a formal specification for WebAssembly are highlighted in the following points:

4.1 **Safety:** WebAssembly is designed to be safe for mobile code on the web, where code originates from untrusted sources. Traditional solutions, such as managed language runtimes, enforce memory safety. WebAssembly achieves safety without compromising on performance, especially for low-level languages like C/C++.
4.2 **Fast Execution:** Low-level code emitted by C/C++ compilers is typically optimized ahead-of-time, allowing it to utilize the full performance of a machine. WebAssembly aims to provide fast execution without the steep performance overhead imposed by managed runtimes and sandboxing techniques.
4.3 **Portability:** WebAssembly addresses the challenge of running code across different device classes, machine architectures, operating systems, and browsers. It aims to be hardware- and platform-independent, allowing applications to run consistently across various environments.
4.4 **Compactness:** Code transmitted over the network should be as compact as possible to reduce load times and improve responsiveness. WebAssembly uses a binary format, which is more compact than transmitting code as JavaScript source, even when minified and compressed.
4.5 **Structured Control Flow:** WebAssembly introduces structured control flow constructs, ensuring that control flow cannot form irreducible loops and simplifying the validation and compilation processes. This approach makes the code more readable and allows for easier debugging.
4.6 **Deterministic Semantics:** WebAssembly aims to provide deterministic semantics across different hardware, ensuring consistent behavior for corner cases such as out-of-range shifts, integer divide by zero, overflow, underflow, and floating-point conversion.
4.7 **Compatibility with Existing Technologies:** WebAssembly is designed to be a cross-browser solution, and its initial version primarily focuses on supporting low-level languages. However, it is an open standard designed for embedding in multiple contexts, and standalone implementations are expected to become available.
The paper emphasizes that WebAssembly is the first industrial-strength language or virtual machine designed with a formal semantics from the start. This not only demonstrates the feasibility of such an approach in the real world but also contributes to a clean design.
Regarding testing, the presence of a formal specification does not imply that WebAssembly implementations should not be tested. Formal specifications provide a clear and rigorous description of the language semantics, but testing is essential to ensure that implementations conform to these specifications and to identify potential bugs, performance issues, or security vulnerabilities specific to the implementation. Testing complements the formal specification by providing practical validation in real-world scenarios.

5. The main advantages of the mechanized specification, as outlined in the text, include:

5.1 **Formal Verification:** The mechanized specification allows for formal verification of the WebAssembly language. Using the Isabelle proof assistant, the author conducts a fully mechanized proof of soundness for the type system. This formal verification enhances confidence in the correctness and reliability of the language.
5.2 **Identification and Resolution of Specification Errors:** The mechanization process revealed several errors in the original WebAssembly specification. These errors were acknowledged and fixed by the working group, indicating that the mechanized effort contributed to improving the accuracy and correctness of the language specification.
5.3 **Executable Interpreter and Type Checker:** The mechanized specification serves as the basis for deriving practical artifacts, including a verified executable interpreter and an executable type checker. These tools are valuable for implementing and checking WebAssembly programs, providing practical applications of the formal model.
5.4 **Eyeball Closeness:** The mechanized specification adheres to the principle of "eyeball closeness," ensuring a line-to-line textual correspondence between the official specification and the mechanization. This design principle improves the clarity and readability of the mechanized specification.
5.5 **Support for Future Language Features:** The mechanization includes support for features like loops with non-empty arguments, even though these features are not yet available due to restrictions in the binary format. This forward-looking approach ensures that the model is ready to accommodate future language developments.

The author verified the specification through the use of Isabelle's proof assistant.
While the mechanized specification enhances the formal verification process and helps identify and resolve issues,
it does not entirely remove the need for testing.
