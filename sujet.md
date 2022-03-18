# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers


https://www.reuters.com/business/autos-transportation/tesla-recalling-nearly-12000-us-vehicles-over-software-communication-error-2021-11-02/

1. In the article the bug is caused by a malfunction of two onboard chips that are disconnected by the software. This issue leads to the faulty detection of negative velocity object that prompted the car to stop. This bug is local and leads to the car stopping in the middle of the road by activating the automatic emergency braking system. This bug affects all the beta user of the Full Self Driving program released by tesla, all kinds of models were affected from model S to Model 3. Tesla had to recall nearly 12 000 vehicules to downgrade their FSD program to the previous version, no accidents were reported related to the issue according to Tesla. The user had to bring their car back to the dealership for a software downgrade.In our opinion with the right scenario they could have simulated the issue and avoid this issue.


https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-796?filter=doneissues

2. SetUniqueList.createSetBasedOnList doesn't add list elements to return value
This bug is local because it involves a human error, a person deleted a line of code that was necessary for the function to work. The function SetUniqueLis.createSetBasedOnList return only null value, the function doesn't add any elements to the set. The solution was to put the deleted line back into the code for the function to work. The contributors also fixed a typo in the code and created a test to ensure that the bug would be detected if it reappears in the future. You can see the new test [here](https://github.com/apache/commons-collections/pull/255/commits/7a62f14b3a8d8b02d84fca41b19b4b202ebd82a0)

3. 
Experiments 

They use a service called Chaos Monkey, which is only active during normal working hours so that the engineers can respond quickly to any incident.
The purpose of this service is to "randomly selects virtual machine instances that host our production services and terminates them". This service can simulate from cutting some servor or some error in injection from other services to an entire server region shutdown.

The usual structure of an experiment : 

1. Start by defining ‘steady state’ as some measurable output of a system that indicates normal behavior.
2. Hypothesize that this steady state will continue in both the control group and the experimental group.
3. Introduce variables that reflect real world events like servers that crash, hard drives that malfunction, network connections that are severed.
4. Try to disprove the hypothesis by looking for a difference in steady state between the control group and the experimental group.
"

Requirements

Chaos engeneering requires to specify : 
* hypotheses
* independant variables
* dependant variables
* context

Variables observed

They observed if the users are able to find some content to watch and successfully watch it.
They created a metric called SPS for streams starts per second. This indicator is the primary data reflecting the general health of the system. They also observe request latency or CPU load. 

Results

The result they obtain is a drop in the SPS in case of a problem or a normal beahvior of the SPS during the course of a day if the change didn't affect the system negatively. If they observe an unexpected change in SPS outside of the normal fluctuation range they know there is a problem.


Netflix is not the only compagny to use the chaos engineering : Amazon, Google, Facebook, Microsoft use a similar strategy.

We could take the exemple of Wikipedia. We could experiment crashing the server and observe the number of research made during the experiments.

4.
The main advantages of having a formal specifications for Web Assembly is that the final code is : 
* Fast : By low leveled code web assembly optimized ahead of time. Native machine code can utilize the full performance of the machine
* Efficient : By using formal semantics the languge is optimized in a way that will allow it to be compact and easyly transimitted through the web media.
* Portable : As Web spans throught a variety of OS and devices this language allows to code application in a platform-independant and hadware-independant way, so that the application will run on all browsers and hadware with the same behavior.
*  Secured : Web assembly use linear memory to store information, this memory is disjointed from the code space. The compiled code cannot corrupt his own environment. That means that even corrupted code can be executed without concern as the program will only mess up is own memory space 
*  Easy to Debug : Even if the final code is in low level assembly code, you can obtain a text version of it to debug which allows developper to understant it more easily
*  Runs on modern browser : as WebAssembly is the result of the cooperation of the biggest leadres on the browser market it can run flawlessy on each of them thus avoiding one the biggest issue of the current most used web language : JavaScript
*  Easy to understand : WebAssembly allox the use of previous Web-unsupported language such as C C++ and the code is then compiled in WebAssembly, so developper can stick to preexisting and wishfully mastered languages

In your opinion, does this mean that WebAssembly implementations should not be tested? 
Even with those safety measures in place, I think that any code should be tested before a proper deployement. The safety measures of WebAssembly garantee a fast validation process helping in code trust process.

5.  
* According to the author of this second paper, what are the main advantages of the mechanized specification? 
Soundness
```
This paper, and
the official specification, both state that the WebAssembly type system enjoys several soundness properties
```
* Did it help improving the original formal specification of the language?

It does not improve it but add new features such as a verified executable interpreter and a type checker 

* What other artifacts were derived from this mechanized specification?

type checker & type interpreter
```
We have also defined a separate verified executable interpreter (§6) and type checker (§5). Like many verified language implementations, these artefacts require integration
with an external parser and linker to run as standalone programs, which introduces an untrusted interface.
```

How did the author verify the specification? 



Does this new specification removes the need for testing?
