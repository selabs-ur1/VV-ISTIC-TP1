# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of _Chaos Engineering_, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested?

5. Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### 1.
In February 1991, during Gulf War a bug occured in the software system of the MIM-104 Patriot. This defense system was used to counter Iraqi-launched Scud missiles.
This bug affected the ability of the system to correctly and precisely track the elapsed time since its activation.
Every time the system was turned on for a certain period, a time anomaly occured. Because the time value was manipulated in binary in one 24-bit register plus a binary fraction in a second 24-bit register, however the fraction 1/10 cannot be perfectly represented by this system. The result of this local bug turned into a failure while attempting to detect the scud missile, and 28 dead soldiers. Fiew tests that could have help to detect this bug :
- Testing the system on a long time of work.
- Testing the system's reaction while reading unsual value.
- Testing its reaction when turned on/off repetitively.

### 2. [Link of page](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-714?filter=allopenissues)

##### Bug:

The issue revolves around PatriciaTrie, a class in the Apache Commons Collections project, mishandling null-terminated strings. In Java, strings don't end with null, meaning "x" (with a length of 1 character) and "x\u0000" (with a length of 2 characters) are distinct. However, PatriciaTrie fails to differentiate between these strings, resulting in unexpected outcomes when using them as keys.
Local bug in Apache Commons Collections' PatriciaTrie: mishandling of null-terminated strings. Appears to impact only this specific class.

##### Solution:

To fix the bug in PatriciaTrie, we need to adjust its functionality to properly recognize the difference between null-terminated and non-null-terminated strings during insertion.

##### Tests:

To ensure the fix works correctly, here are some straightforward tests to add:

- String Differentiation Test:
  Check that PatriciaTrie correctly distinguishes between "x" and "x\u0000" as distinct keys.

- Key Retention Test:
  Ensure that adding new null-terminated keys doesn't result in the loss of existing keys.

- Trie Size Test:
  Verify that the trie size is correct after adding null-terminated strings.

#### 3.

Netflix isn't the only company conducting tests of this kind; Amazon, Google, Microsoft, and Facebook also engage in similar practices. Here's an example of what each company might do in chaos engineering:

- ##### Amazon:

**Chaos Engineering at Amazon Web Services (AWS):** Amazon employs chaos engineering techniques to assess the resilience of its cloud services, including EC2 servers, RDS databases, and other AWS components.
**Fault Simulations:** Amazon simulates failures like instance breakdowns, delays in network requests, or the loss of connections between services to evaluate the system's response.

- ##### Google:

**Chaos Engineering at Google:** Google also embraces chaos engineering to test the reliability of its services, including the Google Cloud Platform (GCP) and other internal services.
**Production Experiments:** Google conducts experiments in production, intentionally introducing failures to assess the resilience of its distributed systems.

- ##### Microsoft:

**Chaos Engineering at Microsoft Azure:** Microsoft incorporates chaos engineering into Azure, its cloud service. This involves resilience testing for virtual machines, databases, and other Azure services.
**Fault Scenarios:** Microsoft engineers simulate fault scenarios such as network errors, delays in requests, or node failures to evaluate application stability.

- ##### Facebook:

**Chaos Engineering at Facebook:** Facebook employs chaos engineering practices to test the reliability of its online social services.
**Fault Injection:** Facebook teams intentionally inject faults, such as errors in communication between services, to observe how the system reacts and recovers.


### 4.

A formal specification provides a clear and consistent definition of the WebAssembly language. It establishes a definitive guide for developers. It gives interoperability, that means code compiled to WebAssembly should behave consistently regardless of where it runs.