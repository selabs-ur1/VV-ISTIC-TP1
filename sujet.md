# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### Question 1 : 
In this article [AMD BUG](https://www.ginjfo.com/actualites/composants/cartes-graphiques/pilotes-graphiques-software-adrenalin-amd-confirme-un-bug-critique-20230306), AMD assume that there is a bug on their graphics driver install. Thee bug is, if you activate factory reset during the AMD driver install, there is a risk that you have to reinstall your OS if a windows update happend at the same time as your AMD driver install. This is a global bug. As specified as before, in the worst case, the customer have to reinstall his OS. Testing the scenario could have helped. It seems obvious that windows will launch an update as the same time as a driver installation. The discovery of this bug before the customer find it could have been better for the AMD driver manager.

### Question 2:
 In this JIRA [Issue](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-814?filter=doneissues), we could assume that the bug is local. The bug is that if the first parameter of the fuction is empty and the second is null it return an empty collection whithout throwing a null pointer exception. The solution in this  [pull request](https://github.com/apache/commons-collections/pull/340), throw an exception when the problem occur. The contributor add new unit test to test this case inside the pull request.

### Question 3:

Netflix conducts two types of chaos engineering experiments :
- **Chaos Monkey Exercises:** Randomly selecting virtual machine instances hosting Netflix's production services and terminating them during normal working hours to test service resilience to individual instance failures.

- **Chaos Kong Exercises:** Simulating the failure of an entire Amazon EC2 region to assess the system's capability to handle the complete loss of a region.

- **Failure Injection Testing (FIT)**: Intentionally causing requests between Netflix services to fail.

The requirement to these experiment are to be able to regenerate/reproduce the system and be able to simulate an event in the system.

From the paper we can extract 2 main variables : 
- **System Health Measurement:** Utilization of the SPS (Stream Starts Per Second) metric to assess the overall system health, measuring the number of users starting video streaming each second.
    
- **Impact on Services:** Observation of effects on specific services, e.g., the impact on the SPS metric after simulating the failure of the bookmarks service.

In all the field tested, the sytem seems to be robust.

Netflix is not the only company performing these experiments Amazon, Google, Microsoft, and Facebook use similar techniques.

In other organizations, these experiments could be mad by cutting of a server and checking if the redundancy is correct. Depending on the scale of the kong experiments could not be always done. Depending the services provided by the company, the metrics could be different. It could be either the number of user over the time or the accessibility of the service.

### Question 4:

Web assembly provide :

**Fast execution:** With Web assembly we could optimize an execution to the lowest level possible.

**Portable :** With web assembly, Code targeting the Web must be hardware and platform-independent to allow applications to run across all browser and hardware types

**Safe** : Managed languages enforce memory safety, preventing programs from compromising userdata or system state.

**Compact :** The code transmitted over the network will be way more compact than a javascript code.

It's not because there is a formal specification for WebAssembly that he should not be tested. IsabelleHOL could be used to proove the WebAssembly and then validate the implementation of this langage.

### Question 5:

In this paper a fully mechanised proof of the soundness of the WebAssembly type system is presented. With this work several issue about WebAssembly have been revealed.

The main advantage of a mechanised specification are :
- **A better formalization :** By mechanizing the specification, the authors ensured a higher level of precision in the language's formal definition.
- **A better Error identification :** By mechanizing the specification, error can be revealed among the WebAssembly specifiation.

This mechanized formalization helped improving WebAssembly specification. It mention in the paper : 

> The original paper formalisation defines 46 reduction rules
for the language. Our mechanisation defines 65;

The author verified the specification through proofs and theorems with the software Isabelle.

The specification upgrade the langage formal definition and corectness, it do not eliminate the need of testing. Testing complete the formal verification.
